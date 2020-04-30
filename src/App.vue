<template>
  <div class="container">
    <SearchBar @termChange="onTermChange" />
    <VideoDetail v-if="selectedVideo" :video="selectedVideo" />
    <VideoList @videoSelect="onVideoSelect" :videos="videos" />
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar";
import VideoList from "./components/VideoList";
import VideoDetail from "./components/VideoDetail";
const API_KEY = "AIzaSyBPbJA_fjfoy1G41OpsMNV0qupkWZp13wk";

export default {
  name: "App",
  data() {
    return {
      videos: [],
      selectedVideo: null
    };
  },
  components: {
    SearchBar,
    VideoList,
    VideoDetail
  },
  methods: {
    onTermChange: function(searchTerm) {
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            key: API_KEY,
            type: "video",
            part: "snippet",
            q: searchTerm
          }
        })
        .then(response => {
          this.videos = response.data.items;
          console.log(response);
        });
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
      console.log(video);
    }
  }
};
</script>
