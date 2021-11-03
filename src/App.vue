<template>
  <div id="app">
    <div class="office">
      <Map @table-clicked="tableClicked" @closed-profile="closeProfile" />
      <SideMenu
        @closed-profile="closeProfile"
        :isUserOpenned="isUserOpened"
        :person="profile"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import people from "@/assets/data/people.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      isUserOpened: false,
      profile: null,
      people: [],
    };
  },
  created() {
    this.loadPeople();
  },
  methods: {
    tableClicked(id) {
      this.isUserOpened = true;
      this.loadProfile(parseInt(id));
    },
    loadPeople() {
      this.people = people;
    },
    loadProfile(id) {
      const res = this.people.find((elem) => {
        if (elem.tableId === id) {
          return true;
        }
        return false;
      });
      if (res) {
        this.profile = res;
      }
    },
    closeProfile(id) {
      this.isUserOpened = false;
      console.log("closing profile", id);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 320px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
