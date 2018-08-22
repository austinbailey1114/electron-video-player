<template>
    <div id="wrapper">
        <div class="draggable">
            {{ (videos.length > 0 ? videos[currentVideoIndex].name : 'None Selected') }}
        </div>
        <div class="video">
            <video
                @click.prevent="handlePlayPause"
                @timeupdate="timeUpdate"
                @ended="onEnded"
                id="video-element"
                :src="(videos.length > 0 ? 'file:///' + videos[currentVideoIndex].path : null)"
                height="100%"
                class="video-item">
                Unable to find video
            </video>
            <div class="menu-bar">
                <div class="progress-container">
                    <div class="progress-parent" @click.prevent="progressClicked">
                        <div class="progress-child" id="progress-child">

                        </div>
                    </div>
                </div>
                <button class="play menu-bar-item" @click="handlePlayPause">{{ playOrPause }}</button>
            </div>
        </div>
        <div class="playlist">
            <div class="playlist-list">
                <input type="file" @change="handleChange($event)" multiple id="file-input">
                <div class="label-container">
                    <label for="file-input" class="file-input-label">
                        Select Files
                    </label>
                </div>
                <div class="playlist-list-container">
                    <div v-for="(video, index) in videos" :class="{ 'is-active' : currentVideoIndex == index }" class="playlist-list-item" @click.prevent="updateVideoSelection(index)">
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
                videoDOMElement: null,
                progressChild: null,
                paused: true,
            }
        },
        mounted() {
            this.videoDOMElement = document.getElementById('video-element')
            this.progressChild = document.getElementById('progress-child')
        },
        methods: {
            open (link) {
                this.$electron.shell.openExternal(link)
            },
            handleChange(event) {
                this.currentVideoIndex = 0
                this.videos = event.target.files
            },
            updateVideoSelection(index) {
                this.currentVideoIndex = index
                this.paused = true
            },
            handlePlayPause() {
                if (!this.paused) {
                    this.videoDOMElement.pause()
                    this.paused = true
                } else {
                    this.videoDOMElement.play()
                    this.paused = false
                }
            },
            onEnded() {
                console.log('ended')
            },
            timeUpdate() {
                this.progressChild.style.width = this.videoDOMElement.currentTime / this.videoDOMElement.duration * 100 + '%'
            },
            progressClicked() {
                
            }
        },
        computed: {
            playOrPause() {
                return (this.paused ? 'Play' : 'Pause')
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
        height: calc(100vh);
        width: 80%;
        background-color: none;
        text-align: center;
    }

    .video-item:hover ~ .menu-bar {
        display: block;
    }

    .menu-bar:hover {
        display: block;
    }

    .video-item {
        background-color: black;
        width: 100%;
        float: left;

    }

    .menu-bar {
        height: 50px;
        background-color: none;
        width: 80%;
        position: absolute;
        display: none;
        bottom: 0;
        transition: 0.2s ease all;
    }

    .menu-bar-item {
        line-height: 46px;
        border: none;
        background-color: none;
        background: none;
        outline: none;
        display: inline-block;
    }

    .play {
        color: white;
        cursor: pointer;
        float: left;
        padding: 0px 10px;
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
        border-top: 1px solid gray;
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

    .is-active {
        background-color: black;
    }

    .progress-container {
        width: 100%;
        height: 4px;
    }

    .progress-parent {
        background-color: gray;
        width: 100%;
        height: 4px;
        cursor: pointer;
    }

    .progress-child {
        background-color: red;
        width: 0%;
        height: 4px;
    }
</style>
