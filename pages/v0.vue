<template>
  <client-only>
  <div id="app">
    <div class="flex justify-center mb-1">
      <h1 class="text-gray-500 text-xs">Treasury Tinder V0</h1>
    </div>
    <Tinder ref="tinder" key-name="id" :queue.sync="queue" :offset-y="10" @submit="onSubmit">
      <template slot-scope="scope">
        <div
          class="pic p-8 flex items-center justify-center"
          :style="{
            'background-image': `url(https://bing.com/th?id=OHR.${scope.data.id}_UHD.jpg&pid=hp&w=720&h=1280&rs=1&c=4&r=0)`
          }"
        >
          <div class="bg-white rounded p-4 m-4" v-if="scope.data.beneficiary">
            <div>Beneficiary</div>
            <div class="font-bold">{{ scope.data.beneficiary }}</div>
            <div class="mt-2">Reason</div>
            <div class="font-bold">{{ scope.data.reason }}<br /></div>
            <div class="mt-2">Value</div>
            <div class="font-bold">{{ scope.data.valueDot }} ({{ scope.data.valueUSD }})</div>
            <!-- {{ scope }} -->
          </div>
          <div class="bg-white rounded p-4 font-bold" v-else>
            <img src="@/assets/images/hot4gov-logo.png" class="w-32 justify-around mb-4">
            You've exhausted your matches for today.<br /><br /> Come back tomorrow, or <a class="underline" target="_blank" href="https://polkadot.js.org/apps/#/treasury/tips">submit your own tip proposal</a> to the Polkadot Treasury.<br /><br /><span class="text-gray-500 font-light italic">Inspired by <a class="underline" target="_blank" href="https://github.com/jonathanpdunne">jonathanpdunne</a>'s idea for Treasury Tinder!</span></span>

          </div>
          
        </div>
      </template>
      <img class="like-pointer" slot="like" src="@/assets/images/like-txt.png">
      <img class="nope-pointer" slot="nope" src="@/assets/images/nope-txt.png">
      <img class="super-pointer" slot="super" src="@/assets/images/super-txt.png">
      <img class="rewind-pointer" slot="rewind" src="@/assets/images/rewind-txt.png">
    </Tinder>
    <div class="btns">
      <img src="@/assets/images/rewind.png" @click="decide('rewind')">
      <img src="@/assets/images/nope.png" @click="decide('nope')">
      <img src="@/assets/images/super-like.png" @click="decide('super')">
      <img src="@/assets/images/like.png" @click="decide('like')">
      <img src="@/assets/images/help.png" @click="decide('help')">
    </div>
  </div>
  </client-only>
</template>

<script>
import Tinder from "vue-tinder";
import source from "@/bing";
import tips from "@/treasurytips";

export default {
  name: "App",
  components: { Tinder },
  data: () => ({
    queue: [],
    offset: 0,
    history: []
  }),
  created() {
    // this.mock();
    this.mockWithTips();
  },
  methods: {
    mock(count = 5, append = true) {
      const list = [];
      for (let i = 0; i < count; i++) {
        list.push({ id: source[this.offset], index: this.offset });
        this.offset++;
      }
      if (append) {
        this.queue = this.queue.concat(list);
      } else {
        this.queue.unshift(...list);
      }
    },
    mockWithTips(count = 15, append = false) {
      const list = [];
      for (let i = 0; i < count; i++) {
        list.push({ 
          id: source[this.offset], 
          index: this.offset, 
          beneficiary: tips[this.offset].beneficiary,
          reason: tips[this.offset].reason,
          valueDot: tips[this.offset].valueDot,
          valueUSD: tips[this.offset].valueUSD,
        });
        this.offset++;
      }
      if (append) {
        this.queue = this.queue.concat(list);
      } else {
        this.queue.unshift(...list);
      }
    },
    onSubmit({ item }) {
      if (this.queue.length < 3) {
        this.mock();
      }
      this.history.push(item);
    },
    async decide(choice) {
      if (choice === "rewind") {
        if (this.history.length) {
          this.$refs.tinder.rewind([this.history.pop()]);
        }
      } else if (choice === "help") {
        // window.open("https://www.google.com");
      } else {
        this.$refs.tinder.decide(choice);
      }
    },
  }
};
</script>

<style>
html,
body {
  height: 100%;
}

body {
  margin: 0;
  background-color: #20262e;
  overflow: hidden;
}

#app .vue-tinder {
  position: absolute;
  z-index: 1;
  left: 0;
  right: 0;
  top: 23px;
  margin: auto;
  width: calc(100% - 20px);
  height: calc(100% - 23px - 65px - 47px - 16px);
  min-width: 300px;
  max-width: 355px;
}

.nope-pointer,
.like-pointer {
  position: absolute;
  z-index: 1;
  top: 20px;
  width: 64px;
  height: 64px;
}

.nope-pointer {
  right: 10px;
}

.like-pointer {
  left: 10px;
}

.super-pointer {
  position: absolute;
  z-index: 1;
  bottom: 80px;
  left: 0;
  right: 0;
  margin: auto;
  width: 112px;
  height: 78px;
}

.rewind-pointer {
  position: absolute;
  z-index: 1;
  top: 20px;
  right: 10px;
  width: 112px;
  height: 78px;
}

.pic {
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
}

.btns {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 30px;
  margin: auto;
  height: 65px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 300px;
  max-width: 355px;
}

.btns img {
  margin-right: 12px;
  box-shadow: 0 4px 9px rgba(0, 0, 0, 0.15);
  border-radius: 50%;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
}

.btns img:nth-child(2n + 1) {
  width: 53px;
}

.btns img:nth-child(2n) {
  width: 65px;
}

.btns img:nth-last-child(1) {
  margin-right: 0;
}
</style>