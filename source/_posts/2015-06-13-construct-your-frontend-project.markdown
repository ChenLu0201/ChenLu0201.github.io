---
layout: post
title: "使用Gulp构建前端工程"
date: 2015-06-13 13:23:08 +0800
comments: true
categories: [前端 构建 前端自动化测试]
---

   为了更好地构建弹性可伸缩应用，越来越多的系统采用前后端分离的结构。后端应用服务化，易于灵活扩展，前端则专注于数据的展现。关于前端开发，有开发经验的工程师应该有这样的经历，随着项目规模的扩大，大量的html, js, css文件将使工程变得难以维护，同时测试也会变得异常的复杂。如果你已经使用了一些工具来优化你的开发，那么你肯定需要或者已经使用了自动化的构建工具帮你处理工程编译，运行测试等重复的工作。本文将系统的介绍前端工程设计使用到的一些技术，并通过Gulp来自动化构建你的前端应用。

接下来我们先看看前端构建会涉及到的哪些内容：


##前端开发常用工具

###Html Template Tools

   在前端开发工程中，编写html是一项烦躁而且枯燥的工作。 大部分标签都要求你在结束时显示的关闭它，这带来重复性劳动，同时使得html文件变得臃肿, 编辑html文件也变成恼人的工作。因此，为了改善程序员的生活，我们会引入一些工具来让编写html template变得更加简洁，快速和美观，比如[Haml](http://haml.info/)(HTML abstraction markup language) 和[Jade](http://jade-lang.com/)(Node Template Engine).下面是使用不同的方式来编写一段html片段的样子:

*HTML*

{% codeblock lang:html %}
<div class="greeting">
	<strong id="message" class="code">Hello World!</strong>
</div>
{% endcodeblock %}

*HAML*

{% codeblock lang:haml %}
.greeting
  %strong{:id => 'message', :class => 'code'} Hello World!
{% endcodeblock %}

*JADE*

{% codeblock lang:jade %}
.greeting
  strong#message.code Hello World!
{% endcodeblock %}

###css precompiler

   关于CSS，它对网站的可用性起着至关重要的作用，很大程度决定了用户是否愿意使用你的网站。但是，维护CSS也是一项浩大的工程，为了让程序员的生活更美好，我们同样可以使用一些工具，来简化编写CSS的工作。比如CSS的预编译器[LESS](http://lesscss.org/)和[SASS(Syntactically Awesome Stylesheets)](http://sass-lang.com/)。下面是一段相同的CSS使用不同工具的写法：


*CSS*

{% codeblock lang:css %}
#search-box .title {
  color: #5f6778;
  font-family: "Roboto Condensed", sans-serif;
  font-size: 16px;
  font-weight: 300;
}

#search-box .content {
  color: #5f6778;
  font-family: "Roboto", sans-serif;
  font-size: 24px;
  font-weight: 500;
}
{% endcodeblock %}

*LESS*

{% codeblock lang:css %}
@title-blue: #166BEC;
@content-gray: #5F6778;
@font-family-roboto: "Roboto", sans-serif;
@font-family-roboto-condensed: "Roboto Condensed", sans-serif;

.font-style(@color, @font-family, @font-size, @font-weight) {
	color: @color;
    font-family: @font-family;
    font-size: @font-size;
  	font-weight: @font-weight;
}

#search-box {
  .title {
      .font-style(@content-gray, @font-family-roboto-condensed, 16px, 300);
  }

  .content {
      .font-style(@content-gray, @font-family-roboto, 24px, 500);
  }
}
{% endcodeblock %}

*SASS*

{% codeblock lang:css %}
$title-blue: #166BEC;
$content-gray: #5F6778;
$font-family-roboto: "Roboto", sans-serif;
$font-family-roboto-condensed: "Roboto Condensed", sans-serif;

@mixin font-style($color, $font-family, $font-size, $font-weight) {
	color: $color;
    font-family: $font-family;
    font-size: $font-size;
  	font-weight: $font-weight;
}

#title {
	@include font-style(@content-gray, @font-family-roboto-condensed, 16px, 300);
}

#content {
	@include font-style(@content-gray, @font-family-roboto, 24px, 500);
}
{% endcodeblock %}


###JS precompiler

   同样，对于JavaScript,我们也可以使用预编译器来帮助我们开发。在我看来，使用编译器有以下三点好处：
1.减少代码量。
2.提高代码的可维护性。
3.编译后的代码是最优实现，有助于提高代码质量。
接下来我们来看看一段JS代码使用[CoffeeScript](http://coffeescript.org/)和[ECMAScript](http://es6-features.org/#Constants)EAMCSCRIPT6来开发分别是怎样的：

*JavaScript*

{% codeblock lang:js %}
angular.module('mainApp').directive('searchCities', function() {
  return {
    restrict: 'E',
    templateUrl: 'app/components/searchCities/partial/searchCities.template.html',
    link: function(scope) {
      var allCities;
      allCities = [
        {
          name: "Beijing",
          info: "Capital of China"
        }, {
          name: "Shanghai",
          info: "Financial Center of China"
        }
      ];
      scope.searchText = '';
      return scope.searchCity = function() {
        return scope.matchedCities = _.filter(allCities, function(city) {
          return city.name.toLowerCase().indexOf(scope.searchText.toLowerCase()) > -1;
        });
      };
    }
  };
});

{% endcodeblock %}

*CoffeeScript*

{% codeblock lang:js %}
angular.module('mainApp').directive('searchCities', () ->
	restrict: 'E',
	templateUrl: 'app/components/searchCities/partial/searchCities.template.html',
	link: (scope) ->
		allCities = [
			{
				name:"Beijing", info:"Capital of China"
			},
			{
				name:"Shanghai", info:"Financial Center of China"
			}
		];
		scope.searchText = '';
		scope.searchCity = () ->
			scope.matchedCities = _.filter(allCities, (city) -> city.name.toLowerCase().indexOf(scope.searchText.toLowerCase()) > -1)
);

{% endcodeblock %}

###JavaScript Unit test

  对于JavaScript的的单元测试，一般有两部分组成。首先我们需要运行和调试测试的测试运行器，常用的有：

- [JSTestDriver](https://code.google.com/p/js-test-driver/)
- [Karma](http://karma-runner.github.io/0.12/index.html)
- [Cucumber.js](https://github.com/cucumber/cucumber-js)

  然后，需要编写能被运行器识别的测试，通常我们选择以下测试框架：

- [JSTestDriver Assertion](https://code.google.com/p/js-test-driver/)
- [Jasmine](http://jasmine.github.io/)
- [QUnit](http://qunitjs.com/)
- [Mocha](http://mochajs.org/)

###E2E Test

  对于开发和维护成本高昂的End2End测试，最近一个项目我们使用了Angular, E2E测试框架使用了[Protractor](https://angular.github.io/protractor/#/),它使用Jasmine的语法来编写测试，同时提供了丰富的API来与页面元素进行交互。下面是一个Protractor测试：
{% codeblock lang:js %}
describe('angularjs homepage todo list', function() {
  it('should add a todo', function() {
    browser.get('https://angularjs.org');

    element(by.model('todoList.todoText')).sendKeys('write first protractor test');
    element(by.css('[value="add"]')).click();

    var todoList = element.all(by.repeater('todo in todoList.todos'));
    expect(todoList.count()).toEqual(3);
  });
});
{% endcodeblock %}

##选择合适的构建工具

上面介绍了在前端开发中常用的一些开发技术。接下来，你需要编译Haml, Less; 压缩并Uglify JavaScripts； 运行测试； 最后发布它。这时，你需要一个构建工具来帮你完成这些重复的工作。

###Gulp VS. Grunt
现在主流的构建工具有[Grunt]()和[Gulp]()。 Grunt出现的更早，它的优点主要体现在以下两个方面：

1. 应用广泛，有完善的社区支持，丰富的插件。
2. 方便的定义任务和之间的依赖。

缺点是：

1. 比较多的文件读写，构建速度慢。
2. 配置优于编码，任务配置的先后顺序，决定了它们之间的依赖关系。

作为后来居上的Gulp，它的特点主要集中在以下几个方面：

1. 易于使用，采用代码优于配置的策略。
2. 它是基于流的构建系统，容易使用。
3. Gulp更高效更高效，它使用[Vinyl](https://github.com/wearefractal/vinyl)来减少文件的读写。
4. 高质量的插件，每个插件专注于做好一件事。

经过以上比较，Gulp看上去更吸引人，如果你真的使用了，实际上的确是这样的。对于一个已使用Grunt的老系统，我们还有理由继续使用，否则应该毫不犹豫的开始使用Gulp。

###使用Gulp来构建工程

介绍了这么多Gulp的特性，相信大家更关心怎么样来使用它。接下来，将介绍在一个使用了Haml, Less, Coffee，Karma，Protractor的前端工程中, 怎样使用Gulp来构建它。

*编译Haml模板*：

{% codeblock lang:js %}
gulp.task('haml', function() {
    gulp.src('src/**/*.haml')
        .pipe(haml())
        .pipe(gulp.dest('src'));
});
{% endcodeblock %}

*编译Less*：

{% codeblock lang:js %}
gulp.task('less', function () {
    gulp.src('src/less/**/main.less')
        .pipe(less())
        .pipe(gulp.dest('src/assets/stylesheets/'));
});
{% endcodeblock %}

*Coffee语法检查 & 编译coffee*：

{% codeblock lang:js %}
gulp.task('coffee-lint', function() {
    gulp.src('src/app/**/*.coffee')
        .pipe(coffeelint())
        .pipe(coffeelint.reporter())
});
gulp.task('coffee', ['coffee-lint'],  function() {
    return gulp.src('src/**/*.coffee')
        .pipe(coffee({bare: true}).on('error', gutil.log))
        .pipe(gulp.dest('src/'));
});
{% endcodeblock %}

*合并和Uglify JavaScript*：

{% codeblock lang:js %}
gulp.task('scripts', ['coffee'], function() {
    gulp.src('src/app/**/*.js')
        .pipe(sourcemaps.init())
        .pipe(concat('all.js'))
        .pipe(uglify())
        .pipe(sourcemaps.write())
        .pipe(gulp.dest('src/assets/javascript/'));
});
{% endcodeblock %}

*单元测试*：

{% codeblock lang:js %}
var testFiles = [
    'src/assets/libs/*.js',
    'src/assets/javascript/*.js',
    'src/**/*.html',
    'test/lib/*.js',
    'test/**/*.spec.js'
];
gulp.task('build', ['haml', 'less', 'scripts']);
gulp.task('test', ['build'], function() {
    gulp.src(testFiles)
        .pipe(karma({
            configFile: 'karma.conf.js',
            action:'run',
            browsers: ['PhantomJS']
        }))
        .on('error', function(err){
            throw err;
        });
});
{% endcodeblock %}

*E2E测试*：

{% codeblock lang:js %}
gulp.task('protractor', ['serve:e2e', 'webdriver-update'], function () {
    gulp.src('e2e/**/*.spec.js')
        .pipe($.protractor.protractor({
            configFile: 'protractor.conf.js'
        }))
        .on('error', function (err) {
            throw err;
        })
        .on('end', function () {
            browserSync.exit();
        });
});
gulp.task('webdriver-update', $.protractor.webdriver_update);
gulp.task('serve:e2e', ['build', 'watch'], function () {
    browserSyncInit([]);
});
var browserSyncInit = function(browser) {
    browserSync({
        notify: false,
        port: 9000,
        server: {
            baseDir: ['src'],
            routes: {
            }
        },
        browser:browser
    });
};
{% endcodeblock %}

更详细的内容，请参考Github上的[工程](https://github.com/ChenLu0201/ng-app-boilerplate.git)。运行之前请先安装[npm](https://www.npmjs.com/)和[bower](http://bower.io/).

##总结

以上，通过梳理前端工程中常用到得技术，我们发现使用一个自动化的构建工具将为我们减少重复性的劳动，并节省大量的时间去做更有益的事。再通过Grunt和Gulp的比较，发现了Gulp的诸多优势。在实际的例子中，使用Gulp来定义任务，跟Grunt里的诸多配置参数不同，Gulp里都是JS代码，更易维护使用。最后，希望本文能激起大家对Gulp的兴趣，更多地细节还需要大家在实际使用中体会。