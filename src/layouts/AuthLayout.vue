/* eslint-disable */
<template>
  <div class="auth-layout">
    <Navbar />
    <div class="main-content">
      <Sidebar />
      <div class="content-area">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '../components/Navbar/NavbarComponent.vue';
import Sidebar from '../components/Sidebar/SidebarComponent.vue';

export default {
  components: {
    Navbar,
    Sidebar,
  },
};
</script>

<style>
.auth-layout {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.main-content {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.content-area {
  flex: 1;
  padding: 20px;
  background-color: #F6F6F6;
  ;
  overflow-y: auto;
}
</style>
