<template>
  <div id="app">
    <div class="section profiles-section">
      <h1>Profiles List</h1>
      <div class="search-bar">
        <label class="label" for="filter">Find profile:</label>
        <input class="input" v-model="searchTerm" @input="findProfile(searchTerm)">
      </div>
      <div class="buttons">
        <button @click="sortAsc">&#9650; </button>
        <button @click="sortDesc"> &#9660;</button>
      </div>

      <ProfileCard
        v-for="profileId in filteredProfiles"
        :key="profileId"
        :profile="profiles[profileId]"
        @add-comment="addComment"
        class="profile"
      />
    </div>

    <div class="section add-profile-section">
      <h1>Add New Profile</h1>
      <div class="form-group">
        <label class="label">Name:</label>
        <input class="input" v-model="newProfile.name">
      </div>
      <div class="form-group">
        <label class="label">Email:</label>
        <input class="input" v-model="newProfile.email">
      </div>
      <div class="form-group">
        <label class="label">Specialisation:</label>
        <input class="input" v-model="newProfile.description">
      </div>
      <button @click="addProfile">Add</button>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard.vue";

export default {
  name: "App",

  components: {
    ProfileCard
  },

  data() {
    return {
      searchTerm: "",
      profiles: {
        1: {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34,
          comments: []
        },
        2: {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28,
          comments: []
        },
        3: {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
          comments: []
        },
        4: {
          id: 4,
          name: "Krzysztof",
          email: "krzysztof@poz.pl",
          description: "Cardiologist",
          likes: 12,
          comments: []
        }
      },
      sortedProfileIds: [1, 2, 3, 4],
      newProfile: {
        name: "",
        email: "",
        description: ""
      }
    };
  },

  computed: {
    filteredProfiles() {
      if (this.searchTerm) {
        return this.sortedProfileIds.filter(id =>
          this.profiles[id].name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }
      return this.sortedProfileIds;
    }
  },

  methods: {
    findProfile(searchTerm) {
      this.searchTerm = searchTerm;
    },

    sortAsc() {
      this.sortedProfileIds.sort((a, b) => this.profiles[a].likes - this.profiles[b].likes);
    },

    sortDesc() {
      this.sortedProfileIds.sort((a, b) => this.profiles[b].likes - this.profiles[a].likes);
    },

    addProfile() {
      if (this.newProfile.name && this.newProfile.email && this.newProfile.description) {
        const newId = Math.max(...this.sortedProfileIds) + 1;
        this.profiles[newId] = { 
          id: newId, 
          ...this.newProfile, 
          likes: 0,
          comments: []
        };
        this.sortedProfileIds.push(newId);
        this.newProfile = { name: "", email: "", description: "" };
      } else {
        alert("All fields are required to add a new profile.");
      }
    },

    addComment(profileId, comment) {
      this.profiles[profileId].comments.push(comment);
    }
  }
};
</script>

<style>
#app {
  font-family: "Roboto", helvetica, arial, sans-serif;
  text-align: center;
  color: #333;
  padding: 20px;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  flex-direction: column;
  position: relative;
  font-size: 1em;
  background-color: #f0f0f0;
}

h1 {
  color: #333;
  margin-bottom: 1em;
}

button {
  display: block;
  padding: 0.5em 1em;
  width: 100%;
  background-color: #4CAF50;
  border: none;
  color: #fff;
  cursor: pointer;
  font-size: 1em;
  font-weight: 600;
  border-radius: 8px;
}

button:hover {
  background-color: #45A049;
}

.content {
  display: flex;
}

.section {
  width: 100%;
  max-width: 600px;
  padding: 2em;
  margin: 1em auto;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.profiles-section {
  margin-bottom: 2em;
}

.search-bar {
  display: flex;
  align-items: center;
  margin-bottom: 1em;
}

.label {
  width: 100px;
  text-align: right;
  margin-right: 1em;
  font-size: 1em;
  color: #555;
}

.input {
  flex: 1;
  padding: 0.5em;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1em;
}

.buttons {
  display: flex;
  justify-content: center;
  margin-top: 1em;
}

.buttons button:first-child {
  margin-right: 1em;
}

.form-group {
  display: flex;
  align-items: center;
  margin-bottom: 1em;
}

.form-group .label {
  width: 120px;
}

.form-group .input {
  flex: 1;
}

.profile {
  margin-top: 1em;
}
</style>
