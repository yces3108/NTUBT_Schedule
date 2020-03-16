<template>
  <div id="app">
    <header>
      <a class="nav-img" href="https://www.facebook.com/ntubt/"><img src="./assets\隊徽.png" width="100px" alt="隊徽"></a>        
      <nav class="nav-menu">
        <ul>
          <router-link to="/"><li class="nav-button">首頁</li></router-link>
          <router-link to="/login"><li class="nav-button">登入</li></router-link>
          <router-link to="/calendar"><li class="nav-button">日程表</li></router-link>
          <router-link to="/schedule"><li class="nav-button">訓練安排</li></router-link>
        </ul>
      </nav>
    </header>
    <router-view />
    <footer>
      <h1>徹底消滅敵人，勇奪大專冠軍。</h1>
    </footer>
  </div>
</template>

<style lang="scss">
/* You can add global styles to this file, and also import other style files */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section, main {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}

/* end reset  */

html {
  font-size: 12px; /* px means 'pixels': the base font size is now 10 pixels high  */
  font-weight: 700;
  font-family: 'Lato', sans-serif;
  font-family: 'Oswald', sans-serif;
  font-family: 'Raleway', sans-serif;
  font-family: 'Microsoft YaHei', sans-serif;
  font-family: 'Noto Sans TC', sans-serif;
}

body {
  //width: 600px;
  //margin: 0 auto;
  background-color: white;
  padding: 10px 20px;
  border: 5px solid rgb(39,60,86);
}
h1 {
  font-size: 20px;
}

.cursor-pointer {
  cursor: pointer;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  //text-align: center;
  color: #2c3e50;
}

header {
  text-align: center;
  font-weight: bold;
}

header a {
    color: inherit; /* 移除超連結顏色 */
    text-decoration: none;  /* 移除超連結底線 */
}
.nav-menu li {
    display: inline-block;
}
.nav-button {
    display: inline-block;
    list-style-type: none;
    width: 100px;
    margin: 5px 10px;
    border: 3px solid rgb(39,60,86);
    border-radius: 5px; 
    padding: 5px 10px;
    color: rgb(39,60,86);
    background-color: white;
    font-size: 1.2rem;
}
.nav-button:hover {
    background-color: rgb(39,60,86);
    color: white;
}
footer {
  text-align: center;
  font-weight: bold;
}

</style>
