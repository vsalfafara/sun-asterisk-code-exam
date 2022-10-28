<template>
  <div>
    <el-input v-model="search" @change="searchVideos"/>
    <el-row>
      <el-col :span="18">
        <div class="container" v-if="topVideo">
          <YouTube 
            :src="`https://www.youtube.com/watch?v=${topVideo.id.videoId}`" 
            ref="youtube"
          />
          <h3>{{topVideo.snippet.title}}</h3>
          <p>{{topVideo.snippet.description}}</p>
        </div>
      </el-col>
      <el-col :span="6">
        <div class="container">
          <div>
            <div class="video" v-for="(video, index) in relatedVideos" :key="index" @click="handleChangeVideo(video)">
              <img :src="video.snippet.thumbnails.default.url" :height="video.snippet.thumbnails.default.height" :width="video.snippet.thumbnails.default.width">
              <p>{{video.snippet.title}}</p>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import { ref } from "vue"
import axios from "axios"
import YouTube from 'vue3-youtube'

const search = ref("")
const topVideo = ref()
const relatedVideos = ref()

const searchVideos = async (term) => {
  const res = await axios.get(`https://www.googleapis.com/youtube/v3/search?key=AIzaSyCD7HE4SDbibgjJn12cQ6cSZ7NukLc6Vxs&part=snippet&q=${term}`)
  topVideo.value = res.data.items[0]
  relatedVideos.value = res.data.items;
}

const handleChangeVideo = async (video) => {
  topVideo.value = video
  const res = await axios.get(`https://www.googleapis.com/youtube/v3/search?key=AIzaSyCD7HE4SDbibgjJn12cQ6cSZ7NukLc6Vxs&part=snippet&relatedToVideoId=${video.id.videoId}&type=video`)
  relatedVideos.value = res.data.items;
}

</script> 