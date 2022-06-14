<template>
  <div class="my-reader">
    <div
      v-for="(li, index) in lists.data"
      class="item"
      @click="open(li.issueId, li.webp)"
      v-show="status"
      :key="index"
    >
      <img
        :src="
          'http://img1-qn.bookan.com.cn/page' +
          li.webp +
          '/6217/6217-' +
          li.issueId +
          '/cover_small.mg'
        "
        :alt="li.issueName"
      />
      <br />
      <span>{{ li.issueName }}</span>
    </div>
    <div id="warp">
      <div id="viewer" :style="{ marginTop: style + 'vh' }">
        <div v-show="!status" v-for="hash in imgArr.data" :key="hash">
          <img
            :src="
              'http://img1-qn.bookan.com.cn/page' +
              imgNum +
              '/6217/6217-' +
              str +
              '/' +
              hash.hash +
              '_big.mg'
            "
            alt=""
            class="page"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "myReader",
  data() {
    return {
      lists: [],
      imgArr: [],
      str: 0,
      style: 0,
      imgNum: 0,
      status: true,
    };
  },
  mounted() {
    fetch(
      "https://api.bookan.com.cn/resource/yearList?resourceType=1&resourceId=6217&year=2022&pageNum=1&limitNum=50"
    )
      .then((res) => {
        return res.json();
      })
      .then((lists) => {
        this.lists = lists;
      });
  },
  methods: {
    open(str, num) {
      this.status = false;
      this.imgNum = num;
      this.str = str;
      fetch(
        "https://api.bookan.com.cn/resource/getHash?resourceType=1&resourceId=6217&issueId=" +
          str +
          "&start=1&end=69"
      )
        .then((res) => {
          return res.json();
        })
        .then((imgArr) => {
          this.imgArr = imgArr;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  margin: 0;
}
.item {
  display: inline-block;
  box-shadow: 0 1px 6px 0 rgba(32, 33, 36, 0.28);
  border-radius: 22px;
  margin: 1em;
  text-align: center;
}
.page {
  height: 2098px;
  display: block;
  margin: 0 auto;
  border-top: 1px grey solid;
}
#viewer {
  text-align: center;
  transition: 0.2s margin-top ease;
}
#warp {
  overflow: hidden;
  background: linen;
}
</style>
