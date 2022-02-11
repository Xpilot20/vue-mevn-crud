<template>
  <div class="search-bar">
    <input type="text" class="from-control" v-model="searchWord" />
    <button class="btn btn-primary from-control" v-on:click="getPlaylistID">
      change
    </button>
  </div>
  <div class="container">
    <div class="row">
      <router-link to="/view" class="nav-link col-6 playlist-card">
        <div
          class="playlist-rank"
          style="
            background-image: linear-gradient(#d6af36, #ffffff);
            width: 100px;
          "
        >
          1
        </div>
        <div class="playlist-info" style="background-color: white">
          <div class="playlist-name">
            Differential Calculus || Basic Mathematics ( BCS - 012) || IGNOU BCA
            1st Semester
          </div>
          <div class="playlis-thumbnail">
            <img
              src="https://i.ytimg.com/vi/f2N3PFZIKF8/mqdefault.jpg"
              alt=""
            />
          </div>
        </div>
      </router-link>
    </div>
  </div>
  <!-- <div class="row justify-content-center">
    <div class="col-md-6">

      <h1>Create Student</h1>
      <form @submit.prevent="handleSubmitForm">
        <div class="form-group">
          <label for="name">Name</label>
          <input
            type="text"
            class="form-control"
            v-model="student.name"
            required
          />
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input
            type="email"
            class="form-control"
            v-model="student.email"
            required
          />
        </div>
        <div class="form-group">
          <label for="phone">Phone</label>
          <input
            type="text"
            class="form-control"
            v-model="student.phone"
            required
          />
        </div>

        <div class="form-group">
          <button class="btn btn-success btn-block">Create</button>
        </div>
      </form>
    </div>
  </div> -->
</template>

<script>
// import $ from "jquery";
import axios from "axios";
export default {
  data() {
    return {
      videoId: [],
      youtubeKey: "AIzaSyAF65tmnK9CCYfFWFUjxVgnqus49BGJ4nQ",
      searchWord: "",
      student: {
        name: "",
        email: "",
        phone: "",
      },
    };
  },
  methods: {
    getPlaylistID() {
      fetch(
        `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${this.searchWord}&key=${this.youtubeKey}&maxResults=10&type=playlist&relevanceLanguage=th&order=rating&regionCode=TH`
      )
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          JSON.stringify(data);
          for (var videoIndex in data["items"]) {
            var videoId = data["items"][videoIndex]["id"]["playlistId"];
            this.videoId[videoIndex] = videoId;
          }
          this.updateData();
        });
      console.log(this.videoId);
    },
    updateData() {
      console.log("works");
    },
    searchPlaylist() {
      console.log(this.className);
      // $("body").addClass(this.className);
    },
    handleSubmitForm() {
      let apiURL = "http://localhost:4000/api/create-student";
      axios
        .post(apiURL, this.student)
        .then(() => {
          this.$router.push("/view");
          this.student = {
            name: "",
            email: "",
            phone: "",
          };
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.search-bar {
  background-color: #cccccc;
  height: 84px;
  display: flex;
  justify-content: center;
  padding: 25px;
}
.playlist-card {
  display: flex;
}
.playlist-media {
  display: flex;
}
.playlist-static {
  font-style: normal;
  font-weight: 500;
  font-size: 18px;
  color: #4e4e4e;
}
.playlist-name {
  font-style: normal;
  font-weight: bold;
  font-size: 18px;
  color: #000000;
}
.playlist-rank {
  font-style: normal;
  font-weight: 500;
  font-size: 50px;
  color: #000000;
}
</style>
