<template>
  <el-container>
    <el-header>
      <h1>OTRP branch ratio calculator</h1>
    </el-header>

    <el-container>
      <el-aside>
        Params
        <!-- <el-input-number v-model="weight.crowded" @change="handleChange" :min="0" :max="1000"></el-input-number>
        <el-input-number v-model="weight.vacant" @change="handleChange" :min="0" :max="1000"></el-input-number>
        <el-input-number v-model="weight.speed" @change="handleChange" :min="-100" :max="100"></el-input-number> -->

        <div class="block">
          <div>citycar_route_weight_crowded</div>
          <span>(0~1000)</span>
          <el-slider 
            v-model="weight.crowded"
            @change="handleChange"
            :min="0"
            :max="1000"
            :step="10"
            show-input
            show-input-controls
            input-size="mini"
          ></el-slider>
        </div>
        <div class="block">
          <div>citycar_route_weight_vacant</div>
          <span>(0~1000)</span>
          <el-slider 
            v-model="weight.vacant"
            @change="handleChange"
            :min="0"
            :max="1000"
            :step="10"
            show-input
            show-input-controls
            input-size="mini"
          ></el-slider>
        </div>
        <div class="block">
          <div>citycar_route_weight_speed</div>
          <span>(-100~100)</span>
          <el-slider 
            v-model="weight.speed"
            @change="handleChange"
            :min="-100"
            :max="100"
            :step="1"
            show-input
            show-input-controls
            input-size="mini"
          ></el-slider>
        </div>
      </el-aside>

      <el-main>
        <div class="content">
          <h2>Four way junction</h2>
          <el-row>
            <el-col :span="8"></el-col>
            <el-col :span="8" class="road bl br">
              <div>North</div>
              <el-input-number 
                v-model="speed.four_way.north"
                @change="handleChange"
                :min="1"
                :max="65535"
                :step="10"
                size="mini"
                controls-position="right"
              ></el-input-number>
              <parts-raito :value="four_raito_north"></parts-raito>
          </el-col>
            <el-col :span="8"></el-col>
          </el-row>
          <el-row>
            <el-col :span="8" class="road bt bb">
              <div>West</div>
              <el-input-number 
                v-model="speed.four_way.west"
                @change="handleChange"
                :min="1"
                :max="65535"
                :step="10"
                size="mini"
                controls-position="right"
              ></el-input-number>
              <parts-raito :value="four_raito_west"></parts-raito>
            </el-col>
            <el-col :span="8" class="road"></el-col>
            <el-col :span="8" class="road bt bb">
              <div>East</div>
              <el-input-number 
                v-model="speed.four_way.east"
                @change="handleChange"
                :min="1"
                :max="65535"
                :step="10"
                size="mini"
                controls-position="right"
              ></el-input-number>
              <parts-raito :value="four_raito_east"></parts-raito>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="8"></el-col>
            <el-col :span="8" class="road bl br"></el-col>
            <el-col :span="8"></el-col>
          </el-row>
        </div>

        <div class="content">
          <h2>Three way junction</h2>
          <el-row>
            <el-col :span="8" class="road bt bb">
              <div>West</div>
              <el-input-number 
                v-model="speed.three_way.west"
                @change="handleChange"
                :min="1"
                :max="65535"
                :step="10"
                size="mini"
                controls-position="right"
              ></el-input-number>
              <parts-raito :value="three_raito_west"></parts-raito>
            </el-col>
            <el-col :span="8" class="road bt"></el-col>
            <el-col :span="8" class="road bt bb">
              <div>East</div>
              <el-input-number 
                v-model="speed.three_way.east"
                @change="handleChange"
                :min="1"
                :max="65535"
                :step="10"
                size="mini"
                controls-position="right"
              ></el-input-number>
              <parts-raito :value="three_raito_east"></parts-raito>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="8"></el-col>
            <el-col :span="8" class="road bl br"></el-col>
            <el-col :span="8"></el-col>
          </el-row>
        </div>

      </el-main>
    </el-container>

    <el-footer height="30px">Copyright <a href="https://twitter.com/128Na">@128Na</a> 2018</el-footer>
  </el-container>
</template>

<script>
import partsRaito from "./components/PartsRaito.vue";
export default {
  name: "app",
  components: {
    "parts-raito": partsRaito
  },
  data() {
    return {
      weight: {
        crowded: 20,
        vacant: 100,
        speed: 0
      },
      speed: {
        four_way: {
          north: 999,
          east: 100,
          west: 100
        },
        three_way: {
          east: 999,
          west: 100
        }
      }
    };
  },
  methods: {
    handleChange(e) {
      console.log(e);
    }
  },
  computed: {
    four_total_wight() {
      return (
        this.four_weight_north + this.four_weight_east + this.four_weight_west
      );
    },
    four_weight_north() {
      return this.weight.vacant + this.speed.four_way.north * this.weight.speed;
    },
    four_weight_east() {
      return this.weight.vacant + this.speed.four_way.east * this.weight.speed;
    },
    four_weight_west() {
      return this.weight.vacant + this.speed.four_way.west * this.weight.speed;
    },
    four_raito_north() {
      return parseInt(this.four_weight_north / this.four_total_wight * 100);
    },
    four_raito_east() {
      return parseInt(this.four_weight_east / this.four_total_wight * 100);
    },
    four_raito_west() {
      return parseInt(this.four_weight_west / this.four_total_wight * 100);
    },

    three_total_wight() {
      return this.three_weight_east + this.three_weight_west;
    },
    three_weight_east() {
      return this.weight.vacant + this.speed.three_way.east * this.weight.speed;
    },
    three_weight_west() {
      return this.weight.vacant + this.speed.three_way.west * this.weight.speed;
    },
    three_raito_east() {
      return parseInt(this.three_weight_east / this.three_total_wight * 100);
    },
    three_raito_west() {
      return parseInt(this.three_weight_west / this.three_total_wight * 100);
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
body > section {
  height: 100vh;
}
header {
  color: #fff;
  background-color: #039be5;
}
aside {
  padding: 20px;
  background-color: #ccc;
}
main {
  background-color: #fff;
}
footer {
  color: #fff;
  background-color: #039be5;
}
a {
  color: #fff;
}
.el-col {
  text-align: center;
  /* border: solid 1px #000; */
  height: 110px;
}
.content {
  max-width: 330px;
}
.el-input-number.el-input-number--mini.is-controls-right {
  width: 90px;
}
.content .road {
  background-color: #ccc;
}
.bt {
  border-top: solid 1px #666;
}
.bb {
  border-bottom: solid 1px #666;
}
.br {
  border-right: solid 1px #666;
}
.bl {
  border-left: solid 1px #666;
}
</style>