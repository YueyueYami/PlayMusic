<style lang="scss" scoped>
    .box {
        width: 100%;
        height: calc(100vh - 100px);
        display: flex;
        color: #fff;
        padding-top: 80px;
        padding-bottom: 80px;
        box-sizing: border-box;

        .left {
            width: 45%;
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            padding-right: 80px;
            box-sizing: border-box;

            .img {
                display: flex;
                flex-direction: column;
                align-items: center;

                img {
                    height: 240px;
                    width: 240px;
                }

                p {
                    width: 180px;
                    height: 40px;
                    text-align: center;
                    line-height: 40px;
                    border: 1px solid #ccc;
                    color: #ccc;
                    font-weight: 100;
                    border-radius: 50px;
                    margin: 30px 0;
                    cursor: pointer;
                    user-select: none;
                }

                span {
                    color: #ccc;
                    font-size: 12px;
                    text-decoration: underline;
                    cursor: pointer;
                    user-select: none;
                }
            }
        }

        .right {
            width: 55%;
            padding-left: 80px;
            box-sizing: border-box;

            .info {
                display: flex;
                flex-direction: column;
                justify-content: space-between;
                height: 15%;

                h1 {
                    font-weight: 300;
                    margin-bottom: 15px;
                }

                .singer {
                    color: #ccc;
                }

                .tosinger {
                    cursor: pointer;
                }
            }

            .lyric {
                color: #ccc;
                height: 85%;
                padding: 20px;
                padding-left: 0;
                box-sizing: border-box;

                ul::-webkit-scrollbar {
                    display: none;
                }

                ul {
                    overflow-y: auto;
                    overflow-x: hidden;
                    height: 100%;

                    :-webkit-scrollbar {
                        display: none;
                    }

                    li {
                        margin: 20px 0
                    }
                }
            }
        }
    }

    footer {
        height: 100px;
        width: 100%;
        position: fixed;
        bottom: 0;
        background: rgba(0, 0, 0, 0.8);
        backdrop-filter: blur(10px);
        display: flex;
        justify-content: center;

        #audio {
            display: none;
        }

        .control {
            height: 100px;
            line-height: 100px;
            color: #fff;
            width: 30%;
            display: flex;
            justify-content: flex-end;
            padding-right: 20px;
            box-sizing: border-box;

            .up,
            .down {
                font-size: 50px;
            }

            .play,
            .suspend {
                font-size: 60px;
                margin: 0 20px;
            }
        }

        .progress {
            width: 40%;
            height: 100%;
            background-color: rgba(204, 204, 204, 0);
            display: flex;
            padding: 0 20px;
            box-sizing: border-box;

            .img {
                width: 100px;
                padding: 10px;
                box-sizing: border-box;

                img {
                    width: 100%;
                    height: 100%;
                }
            }

            .bar {
                flex: 1;
                display: flex;
                flex-direction: column;
                justify-content: center;

                .t-t {
                    display: flex;
                    justify-content: space-between;
                    color: #ccc;
                    font-size: 14px;
                    height: 20px;
                    padding-bottom: 20px;
                }

                .progress-bar {
                    width: 100%;

                    .custom-button {
                        height: 10px;
                        width: 10px;
                        border-radius: 50px;
                        background-color: #fff;
                    }
                }
            }


        }

        .loop {
            width: 30%;
            height: 100%;
            color: #ccc;
            display: flex;
            align-items: center;
            justify-content: flex-start;
            padding-left: 20px;
            box-sizing: border-box;

            i {
                font-size: 24px;
                margin: 0 10px;
            }
        }
    }
</style>

<template>
    <div class="home">
        <div class="box">
            <div class="left">
                <div class="img">
                    <img src="002.png" alt="">
                    <p>下载这首歌</p>
                    <span>外部播放此歌曲 > </span>
                </div>
            </div>
            <div class="right">
                <div class="info">
                    <h1>{{songdata.songname}}</h1>
                    <p>
                        <span class="singer">专辑:&emsp;</span>
                        <span class="tosinger">{{songdata.album}}</span>
                        &emsp;&emsp;
                        <span class="singer">歌手:&emsp;</span>
                        <span class="tosinger">{{songdata.artist}}</span>
                    </p>
                </div>
                <div class="lyric">
                    <ul>
                        <li v-for="(item,i) in songdata.lyric" :key="i">{{item}}</li>
                    </ul>
                </div>
            </div>
        </div>
        <footer>
            <audio id="audio" src="HOYO-MiX _ hanser - Cyberangel.mp3" @timeupdate="time"></audio>
            <div class="control">
                <i @mousedown="fastback" @mouseup="stopback" class="up iconfont icon-24gl-previousCircle"></i>
                <i @click="play" v-show="playshow" class="play iconfont icon-24gl-pauseCircle"></i>
                <i @click="play" v-show="suspendshow" class="suspend iconfont icon-24gl-play"></i>
                <i @mousedown="fastforward" @mouseup="stopfast" class="down iconfont icon-24gl-nextCircle"></i>
            </div>
            <div class="progress">
                <div class="img">
                    <img src="002.png" alt="">
                </div>
                <div class="bar">
                    <div class="t-t">
                        <p class="title">Cyberangel</p>
                        <p class="time">{{currentTime}} / {{totalTime}}</p>
                    </div>
                    <div class="progress-bar">
                        <van-slider v-model="progress" @change="slider">
                            <template #button>
                                <div class="custom-button"></div>
                            </template>
                        </van-slider>
                    </div>
                </div>
            </div>
            <div class="loop">
                <i class="iconfont icon-24gl-volumeZero"></i>
                <i class="iconfont icon-24gl-repeat2"></i>
                <i class="iconfont icon-24gl-shuffle"></i>
                <i class="iconfont icon-24gl-repeatOnce2"></i>
                <i class="iconfont icon-fenxiang"></i>
            </div>
        </footer>
    </div>
</template>
<script>
    export default {
        name: 'HomeView',
        data() {
            return {
                playshow: false,
                suspendshow: true,
                currentTime: '00:00',
                totalTime: '00:00',
                progress: 0,
                songdata: []
            }
        },
        methods: {
            play() {
                this.playshow = !this.playshow;
                this.suspendshow = !this.suspendshow;
                let audio = document.getElementById('audio');
                //获取音频的总时长
                let total = audio.duration
                const min = parseInt(total % 3600 / 60) < 10 ? '0' + parseInt(total % 3600 / 60) : parseInt(total % 3600 / 60)
                const sec = parseInt(total % 3600 % 60) < 10 ? '0' + parseInt(total % 3600 % 60) : parseInt(total % 3600 % 60)
                const time = min + ':' + sec
                this.totalTime = time
                //播放音频
                if (this.playshow) {
                    audio.play();
                } else {
                    audio.pause();
                }
            },
            time(e) {
                //获取音频的当前播放时间
                let current = e.target.currentTime
                const min1 = parseInt(current % 3600 / 60) < 10 ? '0' + parseInt(current % 3600 / 60) : parseInt(current % 3600 / 60)
                const sec1 = parseInt(current % 3600 % 60) < 10 ? '0' + parseInt(current % 3600 % 60) : parseInt(current % 3600 % 60)
                const time1 = min1 + ':' + sec1
                this.currentTime = time1
                let duration = e.target.duration
                //进度
                let progress = current / duration * 100
                this.progress = progress
                if (progress == 100) {
                    this.playshow = !this.playshow;
                    this.suspendshow = !this.suspendshow;
                    this.progress = 0
                }
            },
            slider(e) {
                let audio = document.getElementById('audio');
                audio.currentTime = e / 100 * audio.duration;
            },
            fastforward() {
                window.isClick = true
                clearTimeout(this.timeOut)
                let audio = document.getElementById('audio');
                this.timeOut = setTimeout(function() {
                    this.timer = setInterval(function() {
                        this.progress += 5
                        audio.currentTime = this.progress / 100 * audio.duration;
                        if (this.progress >= 100) {
                            clearTimeout(this.timer)
                        }
                    }.bind(this), 100)
                    window.isClick = false
                }.apply(this), 600)
            },
            stopfast() {
                clearTimeout(this.timer)
                window.isClick = true
            },
            fastback() {
                window.isClick = true
                clearTimeout(this.timeOut)
                let audio = document.getElementById('audio');
                this.timeOut = setTimeout(function() {
                    console.log(1);
                    this.timer = setInterval(function() {
                        this.progress -= 5
                        audio.currentTime = this.progress / 100 * audio.duration;
                        if (this.progress <= 0) {
                            clearTimeout(this.timer)
                        }
                    }.bind(this), 100)
                    window.isClick = false
                }.bind(this), 600)
            },
            stopback() {
                clearTimeout(this.timers)
            },
        },
        created() {
            this.$axios.get('/song.json').then(res => {
                this.songdata = res.data;
            })
        }
    }
</script>