<template>
  <header>
    header
  </header>
  <section class="main-section">
    <aside>
      <ul>
        <li @click="setDashboard">Dashboard</li>
        <li @click="setList">List</li>
      </ul>
    </aside>
    <div class="inner-section">
      <main>
        <div v-if="dashboard"> dashboard </div>
        <div v-else>
          <MyList :permissions="permissions" />
        </div>
      </main>
      <footer>
        footer
      </footer>
    </div>
  </section>
</template>

<script>
import MyList from './components/MyList.vue'
export default {
  name: 'App',
  data() {
    return {
      dashboard: true,
      permissions: ["user.list", "user.list.add", "user.list.update", "user.list.delete"]
    }
  },
  methods: {
    setDashboard() {
      this.dashboard = true
    },
    setList() {
      this.dashboard = false
    }
  },
  components: {
    MyList
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  margin: 0px;
  height: 100%;
}

header {
  height: 10%;
  background: #a3b3c5;
}

aside {
  width: 15%;
  background: #cbd4e0;
}

main {
  height: 80%;
  overflow: auto;
}

footer {
  height: 20%;
  background: #a3b3c5;
}

.inner-section {
  width: 85%;
}
li {
  cursor: pointer;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}

.main-section{
  height: 90%;
  display: flex;
  background: #e4e9f0
  
}
</style>
