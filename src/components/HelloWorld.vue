<template>
  <div>
    <button>提交</button>
    <div style=" display:flex;">
      <div v-for="(item,i) in areaArrTabTitle" :key="i" class="tab_head" @click="tabCC(i)">{{item}}</div>
    </div>
    <div class="tab_content">
      <div v-show="provinceShow">
        <div
          v-for="(provinceItem,provinceIndex) in provinceList"
          :key="provinceIndex"
          @click="decisionProvince(provinceItem)"
        >{{provinceItem.P2}}</div>
      </div>
      <div v-show="cityShow">
        <div
          v-for="(cityItem,cityIndex) in cityListCurr"
          :key="cityIndex"
          @click="decisionCity(cityItem)"
        >{{cityItem.P2}}</div>
      </div>
      <div v-show="areaShow">
        <div v-for="(areaItem,areaIndex) in areaListCurr" :key="areaIndex">{{areaItem.P2}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return {
      provinceShow: true,
      cityShow: false,
      areaShow: false,
      allDateList: [],
      areaArrTabTitle: ["省份", "城市", "地区"],
      provinceList: [],
      cityList: [],
      cityListCurr: [],
      areaList: [],
      areaListCurr: []
    };
  },
  methods: {
    tabCC(i) {
      if (i == 0) {
        (this.provinceShow = true),
          (this.cityShow = false),
          (this.areaShow = false);
      } else if (i == 1) {
        (this.cityShow = true),
          (this.provinceShow = false),
          (this.areaShow = false);
      } else if (i == 2) {
        (this.areaShow = true),
          (this.provinceShow = false),
          (this.cityShow = false);
      }
    },
    async haha() {
      let { data } = await axios({
        method: "post",
        url: "http://120.76.160.41:3000/crossList?page=dmagic_area",
        data: {
          //传给后端的值
          pageSize: 100000
        }
      }).catch(function(error) {
        console.log("异常:" + error);
      });
      //成功后执行的操作
      this.allDateList = data.list;
      console.log("allDateList", this.allDateList);
      this.allDateList.filter(item => {
        if (item.P7.length == 2) {
          this.provinceList.push(item);
        } else if (item.P7.length == 4) {
          this.cityList.push(item);
          // console.log("provinceList",this.provinceList);
        } else if (item.P7.length == 6) {
          this.areaList.push(item);
          //  console.log("provinceList", this.areaList);
        }
      });
    },
    decisionProvince(data) {
      this.cityList.filter(item => {
        if (data.P7 == item.P8) {
          this.cityListCurr.push(item),
            (this.cityShow = true),
            (this.provinceShow = false),
            (this.areaShow = false);
        }
      });
    },
    decisionCity(data) {
      this.areaList.filter(item => {
        if (data.P7 == item.P8) {
          this.areaListCurr.push(item),
            (this.areaShow = true),
            (this.provinceShow = false),
            (this.cityShow = false);
        }
      });
    }
  },
  mounted() {
    this.haha();
window.alert("123");
window.console.log("我爱中国git123gi");

  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tab_head {
  flex-grow: 1;
  background: orange;
  border: 1px solid #000;
  height: 50px;
  text-align: center;
  line-height: 50px;
}
.tab_content {
  border: 1px red solid;
}
</style>
