<template>
    <div id="wrapper">
        <div class="draggable">
            {{ (videos.length > 0 ? videos[currentVideoIndex].name : 'None Selected') }}
        </div>
        <div class="video">
            <video :src="(videos.length > 0 ? 'file:///' + videos[currentVideoIndex].path : null)" height="100%" class="video-item" controls>
                Unable to find video
            </video>
            <div class="menu-bar">

            </div>
        </div>
        <div class="playlist">
            <div class="playlist-list">
                <input type="file" @change="handleChange($event)" multiple id="file-input">
                <div class="label-container">
                    <label for="file-input" class="file-input-label">
                        Select Videos
                    </label>
                </div>
                <div class="playlist-list-container">
                    <div v-for="(video, index) in videos" class="playlist-list-item" @click.prevent="updateVideoSelection(index)">
                        {{ video.name }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'landing-page',
        data() {
            return {
                videos: [],
                currentVideoIndex: 0,
            }
        },
        methods: {
            open (link) {
                this.$electron.shell.openExternal(link)
            },
            handleChange(event) {
                this.currentVideoIndex = 0
                this.videos = event.target.files
                console.log(this.videos)
            },
            updateVideoSelection(index) {
                this.currentVideoIndex = index
                console.log(this.currentVideoIndex)
            }
        }
    }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

    * {
        margin: 0;
        padding: 0;
    }

    body {
        font-family: 'Source Sans Pro', sans-serif;
        overflow-y: hidden;
    }

    .draggable {
        margin: 0;
        padding: 0;
        -webkit-app-region: drag;
        height: 22px;
        width: 100%;
        background-color: #333333;
        text-align: center;
        color: white;
    }

    .video {
        display: inline-block;
        float: left;
        height: calc(100vh - 23px);
        width: 80%;
        background-color: none;
        text-align: center;
    }

    .video-item:hover ~ .menu-bar {
        display: block;
    }

    .video-item {
        background-color: black;
        width: 100%;

    }

    .menu-bar {
        height: 50px;
        background-color: black;
        width: 100%;
        position: absolute;
        display: none;
    }

    .playlist {
        display: inline-block;
        float: right;
        height: calc(100vh);
        width: 20%;
        background-color: #1C1C1C;
        overflow-y: scroll;
    }

    .playlist-list-container {
        margin-top: 20px;
    }

    .playlist-list {
        margin-top: 20px;
        z-index: 10;
    }

    .playlist-list-item {
        color: lightgray;
        font-weight: 200;
        border-bottom: 1px solid gray;
        padding: 15px;
        background-color: #36393A;
        cursor: pointer;
    }

    .playlist-list-item:hover {
        background-color: #DCDCDC;
        color: black;
    }

    #file-input {
        display: none;
    }

    .label-container {
        width: 100%;
        text-align: center;
    }

    .file-input-label {
        padding: 10px;
        color: white;
        border: 1px solid gray;
        border-radius: 4px;
        cursor: pointer;
    }
</style>
