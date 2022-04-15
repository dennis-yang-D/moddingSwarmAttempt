<!doctype html>
<html class="no-js" lang="en">
<!--

Hi there! The non-minified source code is a lot easier to read:
https://github.com/swarmsim/swarm

-->
  <head>
    <meta charset="utf-8">
    <meta http-equiv="Content-Language" content="en-US" />
    <title>Swarm Simulator</title>
    <link rel="shortcut icon" type="image/png" href="favicon.ico">
    <meta name="description" content="An incremental game. Starting with just a few larvae and a small pile of meat, grow a massive swarm of giant bugs.">
    <link rel="canonical" href="https://www.swarmsim.com">
    <meta name="viewport" content="width=device-width">
    <link rel="stylesheet" href="https://s3.amazonaws.com/assets.freshdesk.com/widget/freshwidget.css" />
    <!-- Place favicon.ico and apple-touch-icon.png in the root directory -->
    <!-- build:css(.) styles/vendor.css -->
    <!-- bower:css -->
    <link rel="stylesheet" href="bower_components/angular-hotkeys/build/hotkeys.css" />
    <!-- endbower -->
    <!-- endbuild -->
    <!-- build:css(.tmp) styles/main.css -->
    <link rel="stylesheet" href="styles/main.css">
    <!-- endbuild -->
    <!-- build:css(.tmp) styles/bootstrapdefault.css -->
    <link rel="stylesheet" href="styles/bootstrapdefault.css" />
    <!-- endbuild -->
    <!-- For non-Retina (@1× display) iPhone, iPod Touch, and Android 2.1+ devices: -->
    <link rel="apple-touch-icon-precomposed" href="apple-touch-icon-precomposed.png"><!-- 57×57px -->
    <!-- For the iPad mini and the first- and second-generation iPad (@1× display) on iOS ≤ 6: -->
    <link rel="apple-touch-icon-precomposed" sizes="72x72" href="apple-touch-icon-72x72-precomposed.png">
    <!-- For the iPad mini and the first- and second-generation iPad (@1× display) on iOS ≥ 7: -->
    <link rel="apple-touch-icon-precomposed" sizes="76x76" href="apple-touch-icon-76x76-precomposed.png">
    <!-- For iPhone with @2× display running iOS ≤ 6: -->
    <link rel="apple-touch-icon-precomposed" sizes="114x114" href="apple-touch-icon-114x114-precomposed.png">
    <!-- For iPhone with @2× display running iOS ≥ 7: -->
    <link rel="apple-touch-icon-precomposed" sizes="120x120" href="apple-touch-icon-120x120-precomposed.png">
    <!-- For iPad with @2× display running iOS ≤ 6: -->
    <link rel="apple-touch-icon-precomposed" sizes="144x144" href="apple-touch-icon-144x144-precomposed.png">
    <!-- For iPad with @2× display running iOS ≥ 7: -->
    <link rel="apple-touch-icon-precomposed" sizes="152x152" href="apple-touch-icon-152x152-precomposed.png">
    <!-- For iPhone 6 Plus with @3× display: -->
    <link rel="apple-touch-icon-precomposed" sizes="180x180" href="apple-touch-icon-180x180-precomposed.png">
    <!-- For Chrome for Android: -->
    <meta name="mobile-web-app-capable" content="yes">
    <link rel="icon" sizes="192x192" href="touch-icon-192x192.png">
    <!-- Nice stuff for Metro -->
    <meta name="application-name" content="Swarm Simulator"/>
    <!-- Set a nice background colour for your tile -->
    <meta name="msapplication-TileColor" content="#800080"/>
    <meta name="msapplication-square70x70logo" content="metro_tile-70x70.png"/>
    <meta name="msapplication-square150x150logo" content="metro_tile-150x150.png"/>
    <meta name="msapplication-square310x310logo" content="metro_tile-310x310.png"/>

    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#800080">
  </head>
  <body ng-app="swarmApp">
    <!--[if lt IE 9]>
      <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
    <![endif]-->

    <div class="container">
      <p class="browsehappy safarisupport" style="display:none">Swarm Simulator does not support the Safari browser. If you have trouble, <a href="http://browsehappy.com/">please try Chrome or Firefox</a>.</p>
      <div class="navbar navbar-default" role="navigation" ng-controller="HeaderCtrl">
        <div class="container">
          <!-- Brand and toggle get grouped for better mobile display -->
          <div class="navbar-header">
            <!--button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#navbar">
              <span class="sr-only">Toggle navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
            </button-->
            <a class="navbar-brand page-title" href="#"><span>Swarm Simulator<span></a>
            <a ng-cloak ng-if="env.isDebugEnabled" class="envalert navbar-brand" ng-href="#/debug"> ({{env.name}})</a>
            <a class="navbar-brand" ng-href="#/changelog"><span class="text-muted small" ng-cloak>v{{version}}</span></a>
          </div>
          <div class="navbar-right"><login /></div>

          <!--div class="collapse navbar-collapse" id="navbar">
            <ul class="nav navbar-nav navbar-right">
            </ul>
          </div-->
        </div>
        <div ng-cloak>{{onRender()}}</div>
      </div>

      <!--div class="header"></div-->

      <div ng-cloak ng-controller="LoadSaveCtrl">
        <div class="alert alert-danger alert-dismissible animif" role="alert" ng-if="form.errored && form.export">
          <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
          <p>Oh no! There was a problem loading your saved game. <b>This is a bug.</b></p>
          <p>Here's your saved game data. <b>Save this</b>: once the bug is fixed, you can import this to restore your game.</p>
          <input type="text" class="form-control" readonly style="cursor:auto" ng-model="form.export" ng-click="select($event)">
          <p>The error message was: <code>{{form.error}}</code></p>
          <p>Please <a ng-href="{{contactUrl()}}">report this bug</a>. Thanks!</p>
        </div>
        <div class="alert alert-danger alert-dismissible animif" role="alert" ng-if="form.errored && !form.export">
          <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
          <p>Oh no! There was a problem loading your saved game.</p>
          <p><b>Please make sure <code>{{form.domain}}</code> has permission to set cookies/localstorage in your browser.</b></p>
          <p ng-if="isKongregate()">This problem usually happens when your browser is <a href="https://www.google.com/search?q=how%20to%20enable%20third-party%20cookies" target="_blank">blocking third-party cookies</a>. Swarm Simulator needs this storage to save your game. It's not doing anything evil, I promise.</p>
          <p>The error message was: <code>{{form.error}}</code></p>
          <p>If you think this is a bug, you can <a ng-href="{{contactUrl()}}">report it</a>. Thanks!</p>
        </div>
      </div>
      <div ng-cloak ng-controller="ErrorSavingCtrl">
        <div class="alert alert-danger alert-dismissible animif" role="alert" ng-if="form.errored">
          <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
          <p>Oh no! There was a problem saving your game.</p>
          <p>Here's the data we tried to save. You can import this through the <a href="#/options">options screen</a>.</p>
          <input type="text" class="form-control" readonly style="cursor:auto" ng-model="form.export" ng-click="select($event)">
          <p>The error message was: <code>{{form.error}}</code></p>
        </div>
      </div>
      <div ng-cloak ng-controller="WelcomeBackCtrl">
        <div id="welcomeback" class="alert alert-info alert-dismissible animif" role="alert" ng-if="showWelcomeBack">
          <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
          <p title="Please don't go. The drones need you. They look up to you.">Welcome back! While you were away for {{durationSinceClosed.humanize()}}, your swarm produced:</p>
          <!--span ng-repeat="gain in offlineGains">
            <span ng-if="!$first && $last"> and </span>
              {{gain.val | longnum}}
              <a ng-href="#{{gain.unit.url()}}" ng-click="closeWelcomeBack()">{{gain.unit.unittype.plural}}</a><span ng-if="!$last && offlineGains.length > 2">, </span></span>.</p-->
          <ul>
            <li ng-repeat="gain in offlineGains">
              {{gain.val | longnum}}
              <a ng-href="#{{gain.unit.url()}}" ng-click="closeWelcomeBack()">{{gain.unit.unittype.plural}}</a>
            </li>
          </ul>
        </div>
      </div>
      <div ng-cloak ng-controller="AprilFoolsCtrl">
        <div id="aprilfools-news" class="alert alert-info alert-dismissable animif" role="alert" ng-if="options.aprilFoolsState() != 'off'">
          <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
          <div ng-if="options.aprilFoolsState() === 'on'">
            <span title="Really. Honest. Would I lie to you?" class="small pull-right">{{year}}/04/01</span>
            <div ng-if="!options.isAprilFoolsTheme()">
              <p><strong title="Really. Honest. Would I lie to you?">Exciting changes are coming to Swarm Simulator soon!</strong></p>
              <p>Insects are too icky, so we're changing our name. Also, we'll require graphics to play.</p>
              <p><a href="javascript:" ng-click="options.isAprilFoolsTheme(true)">Try out the upcoming changes now!</a></p>
            </div>
            <div ng-if="options.isAprilFoolsTheme()">
              <p><strong title="Really. Honest. Would I lie to you?">Swarm Simulator is now Kitten Klicker!</strong></p>
              <p>Insects were too icky, so we've changed our name. Also, we now require graphics to play. Thanks to <a target="_blank" href="http://placekitten.com/attribution.html">Placekitten</a> for providing graphics. Enjoy the new game!</p>
              <p><a href="javascript:" ng-click="options.isAprilFoolsTheme(false)">Click here if you hate kittens, you monster.</a></p>
            </div>
          </div>
          <div ng-if="options.aprilFoolsState() === 'after'">
            <span class="small pull-right">{{year}}/04/02</span>
            <p>I hope you enjoyed yesterday's April Fools joke!</p>
            <p><a href="#/cleartheme?themeExtra=@import%20url%28'/static/kittens.css'%29;">Click here to keep the kitten pictures.</a> To remove them later, go to the options screen and click "Clear all extra styling/graphics".</p>
          </div>
        </div>
      </div>

      <tutorial></tutorial>
      <div class="viewwrap"><div ng-view=""><center><img src="images/ajax-loader.gif"></center></div></div>

      <div class="footer">
        <!--div ng-cloak class="alert alert-dismissible alert-warning animif" role="alert">
          <button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
          this is a test footer
        </div-->
      </div>
    </div>

    <debug></debug>
    <div ng-cloak ng-controller="FlashQueueCtrl">
      <div class="achieve achievealert animif" ng-if="achieveQueue.isVisible()">
        <div class="container alert alert-success achievetext">
            <span class="achieveicon hidden-xs glyphicon glyphicon-ok" title="Someday I'll add real achievement icons"></span>
            <span class="achievepoints hidden-xs" ng-if="achieveQueue.get().pointsEarned() > 0">+{{achieveQueue.get().pointsEarned()|number}}</span>
            <span class="achieveicon-xs visible-xs glyphicon glyphicon-ok" title="Someday I'll add real achievement icons"></span>
            <span class="achievepoints-xs visible-xs">+{{achieveQueue.get().pointsEarned()|number}}</span>
            <p>Achievement:</p>
            <a class="alert-link" ng-href="#/achievements">
              <h3>{{achieveQueue.get().type.label}}</h3>
            </a>
            <p class="achievedesc">{{achieveQueue.get().description()}}</p>
            <p><em>{{achieveQueue.get().type.longdesc}}</em></p>
            <button type="button" class="close" data-dismiss="alert" ng-click="achieveQueue.clear()"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
        </div>
      </div>
    </div>

    <script>
      (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
      (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
      m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
      })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

      //ga('create', 'UA-53523462-3', 'auto');
      //ga('send', 'pageview'); // angulartics does this
    </script>
    <script type="text/javascript" src="https://s3.amazonaws.com/assets.freshdesk.com/widget/freshwidget.js"></script>
    <!--[if lt IE 9]>
    <script src="bower_components/es5-shim/es5-shim.js"></script>
    <script src="bower_components/json3/lib/json3.min.js"></script>
    <![endif]-->

    <!-- build:js(.) scripts/vendor.js -->
    <!-- bower:js -->
    <script src="bower_components/angular/angular.js"></script>
    <script src="bower_components/angular-animate/angular-animate.js"></script>
    <script src="bower_components/angular-cookies/angular-cookies.js"></script>
    <script src="bower_components/angular-google-chart/ng-google-chart.js"></script>
    <script src="bower_components/angular-resource/angular-resource.js"></script>
    <script src="bower_components/angular-route/angular-route.js"></script>
    <script src="bower_components/angular-sanitize/angular-sanitize.js"></script>
    <script src="bower_components/angular-touch/angular-touch.js"></script>
    <script src="bower_components/SHA-1/dist/sha1.umd.js"></script>
    <script src="bower_components/angulartics/src/angulartics.js"></script>
    <script src="bower_components/angulartics-google-analytics/lib/angulartics-ga.js"></script>
    <script src="bower_components/jquery/dist/jquery.js"></script>
    <script src="bower_components/bootstrap-sass/assets/javascripts/bootstrap.js"></script>
    <script src="bower_components/bootstrap/dist/js/bootstrap.js"></script>
    <script src="bower_components/decimal.js/decimal.js"></script>
    <script src="bower_components/es5-shim/es5-shim.js"></script>
    <script src="bower_components/favico.js/favico.js"></script>
    <script src="bower_components/flash-cookies/dist/swfstore.min.js"></script>
    <script src="bower_components/jquery-cookie/jquery.cookie.js"></script>
    <script src="bower_components/json3/lib/json3.js"></script>
    <script src="bower_components/lodash/lodash.js"></script>
    <script src="bower_components/lz-string/libs/lz-string.js"></script>
    <script src="bower_components/mathjs/dist/math.js"></script>
    <script src="bower_components/moment/moment.js"></script>
    <script src="bower_components/moment-duration-format/lib/moment-duration-format.js"></script>
    <script src="bower_components/numeral/numeral.js"></script>
    <script src="bower_components/playfab-sdk/PlayFabSdk/src/PlayFab/PlayFabClientApi.js"></script>
    <script src="bower_components/seedrandom/seedrandom.js"></script>
    <script src="bower_components/sjcl/sjcl.js"></script>
    <script src="bower_components/swarm-persist/dist/swarm-persist.js"></script>
    <script src="bower_components/swarm-numberformat/dist/swarm-numberformat.js"></script>
    <script src="bower_components/angular-hotkeys/build/hotkeys.js"></script>
    <script src="bower_components/qs/dist/qs.js"></script>
    <!-- endbower -->
    <!-- endbuild -->

        <!-- build:js({.tmp,app}) scripts/scripts.js -->
        <script src="../node_modules/konami/konami.js"></script>
        <script src="scripts/app.js"></script>
        <script src="scripts/env.js"></script>
        <script src="scripts/spreadsheetpreload/v0.2.js"></script>
        <script src="scripts/services/session.js"></script>
        <script src="scripts/controllers/debug.js"></script>
        <script src="scripts/services/spreadsheet.js"></script>
        <script src="scripts/filters/bignum.js"></script>
        <script src="scripts/services/spreadsheetutil.js"></script>
        <script src="scripts/decorators/exceptionhandler.js"></script>
        <script src="scripts/services/unit.js"></script>
        <script src="scripts/controllers/header.js"></script>
        <script src="scripts/services/game.js"></script>
        <script src="scripts/services/options.js"></script>
        <script src="scripts/controllers/options.js"></script>
        <script src="scripts/services/upgrade.js"></script>
        <script src="scripts/services/util.js"></script>
        <script src="scripts/services/effect.js"></script>
        <script src="scripts/services/analytics.js"></script>
        <script src="scripts/controllers/changelog.js"></script>
        <script src="scripts/services/command.js"></script>
        <script src="scripts/services/statistics.js"></script>
        <script src="scripts/controllers/statistics.js"></script>
        <script src="scripts/services/timecheck.js"></script>
        <script src="scripts/services/flashqueue.js"></script>
        <script src="scripts/controllers/flashqueue.js"></script>
        <script src="scripts/controllers/achievements.js"></script>
        <script src="scripts/services/achievement.js"></script>
        <script src="scripts/directives/cost.js"></script>
        <script src="scripts/controllers/main.js"></script>
        <script src="scripts/services/tab.js"></script>
        <script src="scripts/directives/buyunit.js"></script>
        <script src="scripts/directives/tabs.js"></script>
        <script src="scripts/directives/tutorial.js"></script>
        <script src="scripts/directives/unit.js"></script>
        <script src="scripts/directives/description.js"></script>
        <script src="scripts/controllers/loadsave.js"></script>
        <script src="scripts/services/favico.js"></script>
        <script src="scripts/directives/debug.js"></script>
        <script src="scripts/services/kongregate.js"></script>
        <script src="scripts/services/seedrand.js"></script>
        <script src="scripts/services/backfill.js"></script>
        <script src="scripts/filters/moment.js"></script>
        <script src="scripts/services/feedback.js"></script>
        <script src="scripts/controllers/contact.js"></script>
        <script src="scripts/controllers/errorsaving.js"></script>
        <script src="scripts/controllers/cleartheme.js"></script>
        <script src="scripts/services/storage.js"></script>
        <script src="scripts/controllers/importsplash.js"></script>
        <script src="scripts/services/remotesave.js"></script>
        <script src="scripts/controllers/chart.js"></script>
        <script src="scripts/services/parsenumber.js"></script>
        <script src="scripts/directives/login.js"></script>
        <script src="scripts/services/user_api.js"></script>
        <script src="scripts/controllers/login.js"></script>
        <script src="scripts/controllers/debugapi.js"></script>
        <script src="scripts/controllers/decimallegend.js"></script>
        <script src="scripts/directives/converttonumber.js"></script>
        <script src="scripts/directives/playfab.js"></script>
        <script src="scripts/directives/playfabauth.js"></script>
        <script src="scripts/directives/playfaboptions.js"></script>
        <script src="scripts/services/playfab.js"></script>
        <script src="scripts/controllers/news-archive.js"></script>
        <script src="scripts/directives/news-modal.js"></script>
        <script src="scripts/services/mtx.js"></script>
        <script src="scripts/directives/buyunit-input.js"></script>
        <script src="scripts/directives/crystaltimer.js"></script>
        <script src="scripts/directives/dropdown-menu-auto-direction.js"></script>
        <script src="scripts/controllers/export.js"></script>
        <script src="scripts/directives/domainchange.js"></script>
        <script src="scripts/directives/elm.js"></script>
        <!-- endbuild -->
</body>
<script>
// this is hideous, but we're broken in safari, don't have a mac to fix it with, and kong staff insists on this
var ua = window && window.navigator && window.navigator.userAgent
var isSafari = ua.match(/Version\/(\S+).*?Safari\//);
if (isSafari) {
  jQuery('.safarisupport').style({'display':'initial'});
}
</script>
<script>
// https://codelabs.developers.google.com/codelabs/add-to-home-screen/#5
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/service-worker.js').then(function(reg){
      console.log("service worker registered");
    }).catch(function(err) {
      console.warn("service worker registration failed", err)
    });
  });
}
</script>
</html>

<!-- MEOW -->
