<template>
    <div class="ui-loadmore">
        <div v-if="showTop.show" class="ui-loadmore-top" :style="showTop.style">
            <span class="ui-loadmore-top-text">{{showTop.text}}</span>
        </div>
        <slot></slot>
        <div v-if="up" class="ui-loadmore-bottom" @click="loadmore">
            <span class="ui-loadmore-bottom-text" v-if="(loading||allLoaded)?(loading?bottomLoadingText:bottomNomoreText):bottomText" v-text="(loading||allLoaded)?(loading?bottomLoadingText:bottomNomoreText):bottomText"></span>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        pull: {
            type: Boolean,
            default: false
        },
        topPullText: {
            type: String,
            default: "下拉刷新"
        },
        topText: {
            type: String,
            default: "松手刷新"
        },
        bottomText: {
            type: String,
            default: "加载更多"
        },
        bottomLoadingText: {
            type: String,
            default: "加载中..."
        },
        bottomNomoreText: {
            type: String,
            default: "暂无更多了~"
        },
        loading: {
            type: Boolean,
            default: false
        },
        allLoaded: {
            type: Boolean,
            default: false
        },
        topDistance: {
            type: Number,
            default: 40
        },
        pull: {
            type: Boolean,
            default: true
        },
        up: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            showTop: {
                show: false,
                style: {},
                text: this.topText
            },
            showBottom: {
                show: false,
                style: {}
            },
            start: {
                Y: 0
            },
            end: {
                Y: 0
            },
            scrollElement: null
        };
    },
    mounted() {
        if (this.pull) {
            let currentNode = this.$el.parentElement;
            let find = false;
            while (!find && currentNode && currentNode.tagName !== "HTML" && currentNode.tagName !== "BODY" && currentNode.nodeType === 1) {
                let overflowY = document.defaultView.getComputedStyle(currentNode).overflow;
                if (overflowY === "scroll" || overflowY === "auto") {
                    find = true;
                } else {
                    currentNode = currentNode.parentNode;
                }
            }
            this.scrollElement = find ? currentNode : window;

            this.scrollElement.addEventListener("touchstart", this.touchstart);
            this.scrollElement.addEventListener("touchmove", this.touchmove);
            this.scrollElement.addEventListener("touchend", this.touchend);
        }
    },
    methods: {
        touchstart($event) {
            if (this.scrollElement.scrollTop != 0) return;
            this.start.Y = $event.targetTouches[0].pageY;
        },
        touchmove($event) {
            if (this.scrollElement.scrollTop != 0) return;
            if ($event.targetTouches[0].pageY > this.start.Y) {
                $event.preventDefault();
                $event.stopPropagation();
                // this.scrollElement.style.overflow = "hidden";
                this.showTop.show = true;
                this.showTop.style = { height: ($event.targetTouches[0].pageY - this.start.Y) / 2 + "px" };
                if ($event.targetTouches[0].pageY - this.start.Y < this.topDistance * 2) {
                    this.showTop.text = this.topPullText;
                } else {
                    this.showTop.text = this.topText;
                }
            } else {
                this.showTop.show = false;
                this.showTop.style = { height: "0" };
            }
        },
        touchend($event) {
            if (this.scrollElement.scrollTop != 0) return;
            // this.scrollElement.style.overflow = "auto";
            if ($event.changedTouches[0].pageY - this.start.Y > 10) {
                $event.preventDefault();
                $event.stopPropagation();
            }
            if ($event.changedTouches[0].pageY - this.start.Y >= this.topDistance * 2) {
                if (this.$el.style.display != "none") {
                    this.$emit("pull");
                }
            }
            this.showTop.style = { transition: "all 0.3s", height: "0" };
            setTimeout(() => {
                this.showTop.show = false;
            }, 100);
        },
        loadmore() {
            if (this.loading || this.allLoaded) return;
            this.$emit("loadmore");
        }
    }
};
</script>

<style lang="less">
.ui-loadmore {
    .ui-loadmore-top {
        height: 0;
        overflow: hidden;
        text-align: center;
        .ui-loadmore-top-text {
            display: inline-block;
            padding: 15px;
            font-size: 13px;
            color: #666;
        }
    }
    .ui-loadmore-bottom {
        text-align: center;
        .ui-loadmore-bottom-text {
            display: inline-block;
            padding: 30px;
            font-size: 13px;
            color: #666;
        }
    }
}
</style>