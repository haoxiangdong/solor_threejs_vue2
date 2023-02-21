<template>
    <div>
        11111111111111
        <div class="controls-box">
      <section>
        <el-row>
          <el-checkbox v-model="properties.visible">visible</el-checkbox>
        </el-row>
        <el-row>
          <el-checkbox v-model="properties.transparent">transparent</el-checkbox>
        </el-row>
        <el-row>
          <el-col :span="8" class="label-col"><label> emissive</label></el-col>
          <el-col :span="16">
            <div @click="inputClick">
              <el-input :value="properties.color"></el-input>
            </div>
            <div v-show="isShowColors" class="color-select-layer">
              <sketch-picker v-model="properties.color" @input="colorChange"></sketch-picker>
            </div>
          </el-col>
        </el-row>
        <el-row>
          <div v-for="(item, key) in properties" :key="key">
            <div v-if="item && item.name != undefined">
              <el-col :span="8">
                <span class="vertice-span">{{ item.name }}</span>
              </el-col>
              <el-col :span="13">
                <el-slider v-model="item.value" :min="item.min" :max="item.max" :step="item.step"
                  :format-tooltip="formatTooltip"></el-slider>
              </el-col>
              <el-col :span="3">
                <span class="vertice-span">{{ item.value }}</span>
              </el-col>
            </div>
          </div>

        </el-row>
        <el-row>
          <el-col :span="8" class="label-col"><label>side</label></el-col>
          <el-col :span="16">
            <el-select v-model="properties.side" placeholder="请选择">
              <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="8" class="label-col"><label>mesh</label></el-col>
          <el-col :span="16">
            <el-select v-model="properties.selectMesh" placeholder="请选择">
              <el-option v-for="item in selectMeshOptions" :key="item.value" :label="item.label" :value="item.value">
              </el-option>
            </el-select>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="8" class="label-col">当前组件{{ this.page }}</el-col>
        </el-row>
        <el-row>
          <el-col :span="8"><el-button type="primary" @click="handleCut">切换组件</el-button></el-col>
        </el-row>
      </section>

    </div>
    </div>
</template>

<script>
import { Sketch } from 'vue-color'
export default {
    name: 'ModelImportTestComponets',
    components: {
    'sketch-picker': Sketch
  },
    data() {
        return {
            options: [
        {
          value: 'front',
          label: 'front'
        },
        {
          value: 'back',
          label: 'back'
        },
        {
          value: 'double',
          label: 'double'
        }
      ],
      selectMeshOptions: [
        {
          value: 'cube',
          label: 'cube'
        },
        {
          value: 'sphere',
          label: 'sphere'
        },
        {
          value: 'plane',
          label: 'plane'
        }
      ],

      properties: {
        opacity: {
          name: 'opacity',
          value: 1,
          min: 0,
          max: 1,
          step: 0.1
        },
        color: '#194977',
        selectMesh: 'sphere',
        side: 'double',
        transparent: false,
        wrapAround: false,
        visible: true
      }, 
      page:'',

        };
    },
    created(){
    },
    mounted() {
        
    },

    methods: {
        handleCut(){
      if (this.page==1) {
        this.page=2
        this.$router.push({
          path:'/about',
        })
      }else{
        this.page=1
        this.$router.push({
          path:'/',

        })
      }
      
    },
    formatTooltip(val) {
      return val
    },
    inputClick() {
      this.isShowColors = !this.isShowColors
    },
    colorChange(val) {
      this.properties.color = val.hex
    }, 
    },
};
</script>

<style scoped>
#container {
  position: absolute;
  width: 100%;
  height: 100%;
}

.controls-box {
  position: absolute;
  right: 5px;
  top: 5px;
  width: 300px;
  padding: 10px;
  background-color: #fff;
  border: 1px solid #c3c3c3;
}

.label-col {
  padding: 8px 5px;
}

.color-select-layer {
  position: relative;
  left: -20px;
  padding: 15px 0;
}

.vertice-span {
  line-height: 38px;
  padding: 0 2px 0 10px;
}
</style>