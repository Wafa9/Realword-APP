<template>
  <div id="app">
    <RwvHeader />
    <router-view></router-view>
    <RwvFooter />
  </div>
</template>

<script>
import RwvHeader from "@/components/TheHeader";
import RwvFooter from "@/components/TheFooter";

export default {
  name: "App",
  components: {
    RwvHeader,
    RwvFooter
  }
};
</script>

<style>
html {
  font-family: "Source Code Pro", monospace;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.home-page .banner {
  background: #b6e6bd;
}
.home-page .sidebar {
  background: #ecf4f3;
  position: fixed;
  display: block;
  padding-right: 3px;
  box-shadow: 0 2.8px 2.2px rgba(0, 0, 0, 0.034),
    0 6.7px 5.3px rgba(0, 0, 0, 0.048), 0 12.5px 10px rgba(0, 0, 0, 0.06),
    0 22.3px 17.9px rgba(0, 0, 0, 0.072), 0 41.8px 33.4px rgba(0, 0, 0, 0.086),
    0 100px 80px rgba(0, 0, 0, 0.12);
}
.article-preview {
  background: #ecf4f3;
  border-radius: 10px;
  margin-bottom: 10px;
  padding: 25px;
  box-shadow: 0 2.8px 2.2px rgba(0, 0, 0, 0.034),
    0 6.7px 5.3px rgba(0, 0, 0, 0.048), 0 41.8px 33.4px rgba(0, 0, 0, 0.086),
    0 100px 80px rgba(0, 0, 0, 0.12);
}
.tag-default {
  background: #8fc0a9;
}
.article-page .banner {
  background: #b6e6bd;
}
.profile-page .user-info {
  background: #b6e6bd;
}
</style>
