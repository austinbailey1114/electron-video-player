<template>
    <div id="wrapper">
        <div class="draggable">

        </div>
        <div class="video">
            <video :src="'file:///' + videoFile.path" height="100%" class="video-item">
                Unable to find video
            </video>
            <div class="menu-bar">

            </div>
        </div>
        <div class="playlist">
            <div class="playlist-list">
                <input type="file" @change="handleChange($event)">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'landing-page',
        data() {
            return {
                videoFile: {
                    path: 'abc'
                }
            }
        },
        methods: {
            open (link) {
                this.$electron.shell.openExternal(link)
            },
            handleChange(event) {
                this.videoFile = event.target.files[0]
                console.log(this.videoFile)
            },
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
        /* overflow-y: hidden; */
    }

    .draggable {
        margin: 0;
        padding: 0;
        -webkit-app-region: drag;
        height: 22px;
        width: 100%;
        position: absolute;
        background-color: pink;
    }

    .video {
        display: inline-block;
        float: left;
        height: calc(100vh);
        width: 80%;
        background-color: black;
        text-align: center;
    }

    .video-item:hover ~ .menu-bar {
        display: block;
    }

    .video-item {
        background-color: orange;
        width: 100%;

    }

    .menu-bar {
        height: 50px;
        background-color: orange;
        width: 100%;
        position: absolute;
        display: none;
    }

    .playlist {
        display: inline-block;
        float: right;
        height: calc(100vh);
        width: 20%;
        background-color: red;
    }

    .playlist-list {
        margin-top: 25px;
        z-index: 10;
    }
</style>
