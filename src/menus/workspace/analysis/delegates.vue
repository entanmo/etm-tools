<template>
  <div class="wrapper-content">
    <div class="head-area">
      <a-card :bordered="false">
        <a-row>
          <a-col :sm="12"
                 :xs="36">
            <headinfo title="当前范围块矿工数"
                      :content="totalDelegates"
                      :bordered="true" />
          </a-col>
          <a-col :sm="12"
                 :xs="36">
            <headinfo title="统计范围"
                      :content="range" />
          </a-col>
        </a-row>
      </a-card>
    </div>
    <div class="body-area">
      <div class="chart">
        <div class="chart-top">
          <div class="top-title">
            <h4>矿工统计：</h4>
          </div>
          <div>
            统计轮初始高度：
            <a-input-number :min="1"
                            v-model="startHeight"
                            :disabled="!disabled"
                            size="small" />
          </div>
          <div>
            统计轮长度：
            <a-input-number :min="101"
                            v-model="len"
                            :disabled="!disabled"
                            size="small" />
          </div>
          <div class="top-switch">
            <a-switch @change="onSwitch"
                      checkedChildren="统计开"
                      unCheckedChildren="统计关">
            </a-switch>
          </div>
        </div>
        <div class="chart-center">
          <visersmoothline v-show="!disabled"
                           :vdata="vdata" />
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import Server from "@/scripts/server.js";
import visersmoothline from "@/components/viser/ViserSmoothLine";
import headinfo from "@/components/tool/HeadInfo";

export default {
  data() {
    return {
      vdata: {
        data: [],
        height: 250,
        scale: [
          {
            dataKey: "count",
            tickInterval: 1,
            min: 0
          }
        ],
        axis: [
          {
            key: "username"
          },
          {
            key: "count",
            color: "blue",
            color2: "green"
          }
        ]
      },
      disabled: true,
      totalDelegates: 0,
      range: "0--0",
      startHeight: 1,
      len: 505
    };
  },
  components: {
    visersmoothline,
    headinfo
  },
  methods: {
    onSwitch(checked) {
      this.disabled = !checked;

      if (checked) {
        this.getBlocks(this.startHeight - 1, this.len);
      } else {
        this.vdata.data = [];
        this.totalDelegates = 0;
        this.range = "0--0";
      }
    },
    async getBlocks(startHeight, len) {
      let blocks = [];
      let server = new Server();
      for (let i = 0; i < len; i += 100) {
        let limit = 100;
        let offset = startHeight + i;
        if (i + 100 > len) {
          limit = len - i;
        }
        let res = await server.get("api/blocks", { limit, offset });
        blocks = blocks.concat(res.blocks);
      }

      let generators = {};
      for (let i = 0; i < blocks.length; i++) {
        if (blocks[i].height === 1) {
          continue;
        }
        let generator = blocks[i].generatorPublicKey;
        if (generators[generator]) {
          generators[generator] += 1;
        } else {
          generators[generator] = 1;
        }
      }
      let keys = Object.keys(generators);
      for (let i = 0; i < keys.length; i++) {
        let key = keys[i];
        let res = await server.get("/api/delegates/get", { publicKey: key });
        let username = "null";
        if (res.delegate && res.delegate.username) {
          username = res.delegate.username;
        }
        this.vdata.data.push({
          username: username,
          count: generators[key]
        });
      }

      this.totalDelegates = keys.length;
      this.range = this.startHeight + "--" + (this.startHeight + this.len - 1);
    }
  }
};
</script>

<style lang="less" scoped>
.wrapper-content {
  padding-bottom: 20px;

  .head-area {
    padding-bottom: 20px;
  }
  .body-area {
    .chart {
      height: 300px;
      background-color: #fff;

      .chart-top {
        height: 50px;
        padding: 10px;
        display: flex;
        justify-content: space-between;

        .top-title {
          width: 100px;
        }
      }
    }
  }
}
</style>
