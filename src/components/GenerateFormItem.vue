<template>
  <div>
    <template v-if="widget.type==='title'">
      <div>
        <div
          v-if="widget.type==='title'"
          :style="{
            fontSize: widget.options.style.fontSize + 'px',
            fontWeight: widget.options.style.bold ? 'bold' : 'normal',
            marginTop: widget.options.style.marginTop + 'px',
            marginBottom: widget.options.style.marginBottom + 'px',
            marginLeft: widget.options.style.marginLeft + 'px',
          }"
        >
          {{ widget.name }}
        </div>
      </div>
    </template>
    <template v-else>
      <el-form-item :label="widget.name" :prop="widget.model" :class="{[widget.options.customClass]: true}">
        <template v-if="widget.type == 'input'">
          <el-input
            v-if="widget.options.dataType == 'number' || widget.options.dataType == 'integer' || widget.options.dataType == 'float'"
            v-model.number="dataModel"
            :type="widget.options.dataType"
            :placeholder="widget.options.placeholder"
            :style="{width: widget.options.width}"
            :disabled="widget.options.disabled"
          />
          <el-input
            v-else
            v-model="dataModel"
            :type="widget.options.dataType"
            :disabled="widget.options.disabled"
            :placeholder="widget.options.placeholder"
            :style="{width: widget.options.width}"
          />
        </template>

        <template v-if="widget.type == 'textarea'">
          <el-input
            v-model="dataModel"
            type="textarea"
            :rows="5"
            :disabled="widget.options.disabled"
            :placeholder="widget.options.placeholder"
            :style="{width: widget.options.width}"
          />
        </template>

        <template v-if="widget.type == 'number'">
          <el-input-number
            v-model="dataModel"
            :style="{width: widget.options.width}"
            :step="widget.options.step"
            controls-position="right"
            :disabled="widget.options.disabled"
          />
          <span style="margin-left:4px">{{ widget.options.unit }}</span>
        </template>

        <template v-if="widget.type == 'radio'">
          <el-radio-group
            v-model="dataModel"
            :style="{width: widget.options.width}"
            :disabled="widget.options.disabled"
          >
            <el-radio
              v-for="(item, index) in (widget.options.remote ? widget.options.remoteOptions : widget.options.options)"
              :key="index"
              :style="{display: widget.options.inline ? 'inline-block' : 'block'}"
              :label="item.value"
            >
              <template v-if="widget.options.remote">{{ item.label }}</template>
              <template v-else>{{ widget.options.showLabel ? item.label : item.value }}</template>
            </el-radio>
          </el-radio-group>
        </template>

        <template v-if="widget.type == 'checkbox'">
          <el-checkbox-group
            v-model="dataModel"
            :style="{width: widget.options.width}"
            :disabled="widget.options.disabled"
          >
            <el-checkbox
              v-for="(item, index) in (widget.options.remote ? widget.options.remoteOptions : widget.options.options)"
              :key="index"
              :style="{display: widget.options.inline ? 'inline-block' : 'block'}"
              :label="item.value"
            >
              <template v-if="widget.options.remote">{{ item.label }}</template>
              <template v-else>{{ widget.options.showLabel ? item.label : item.value }}</template>
            </el-checkbox>
          </el-checkbox-group>
        </template>

        <template v-if="widget.type == 'time'">
          <el-time-picker
            v-model="dataModel"
            :is-range="widget.options.isRange"
            :placeholder="widget.options.placeholder"
            :start-placeholder="widget.options.startPlaceholder"
            :end-placeholder="widget.options.endPlaceholder"
            :readonly="widget.options.readonly"
            :disabled="widget.options.disabled"
            :editable="widget.options.editable"
            :clearable="widget.options.clearable"
            :arrow-control="widget.options.arrowControl"
            :value-format="widget.options.format"
            :style="{width: widget.options.width}"
          />
        </template>

        <template v-if="widget.type=='date'">
          <el-date-picker
            v-model="dataModel"
            :type="widget.options.type"
            :placeholder="widget.options.placeholder"
            :start-placeholder="widget.options.startPlaceholder"
            :end-placeholder="widget.options.endPlaceholder"
            :readonly="widget.options.readonly"
            :disabled="widget.options.disabled"
            :editable="widget.options.editable"
            :clearable="widget.options.clearable"
            :value-format="widget.options.timestamp ? 'timestamp' : widget.options.format"
            :format="widget.options.format"
            :style="{width: widget.options.width}"
          />
        </template>

        <template v-if="widget.type =='rate'">
          <el-rate
            v-model="dataModel"
            :max="widget.options.max"
            :disabled="widget.options.disabled"
            :allow-half="widget.options.allowHalf"
          />
        </template>

        <template v-if="widget.type == 'color'">
          <el-color-picker
            v-model="dataModel"
            :disabled="widget.options.disabled"
            :show-alpha="widget.options.showAlpha"
          />
        </template>

        <template v-if="widget.type == 'select'">
          <el-select
            v-model="dataModel"
            :disabled="widget.options.disabled"
            :multiple="widget.options.multiple"
            :clearable="widget.options.clearable"
            :placeholder="widget.options.placeholder"
            :style="{width: widget.options.width}"
            :filterable="widget.options.filterable"
          >
            <el-option v-for="item in (widget.options.remote ? widget.options.remoteOptions : widget.options.options)" :key="item.value" :value="item.value" :label="widget.options.showLabel || widget.options.remote?item.label:item.value" />
          </el-select>
        </template>

        <template v-if="widget.type=='switch'">
          <el-switch
            v-model="dataModel"
            :disabled="widget.options.disabled"
          />
        </template>

        <template v-if="widget.type=='slider'">
          <el-slider
            v-model="dataModel"
            :min="widget.options.min"
            :max="widget.options.max"
            :disabled="widget.options.disabled"
            :step="widget.options.step"
            :show-input="widget.options.showInput"
            :range="widget.options.range"
            :style="{width: widget.options.width}"
          />
        </template>

        <template v-if="widget.type == 'editor'">
          <vue-editor
            v-model="dataModel"
            :style="{width: widget.options.width}"
          />
        </template>

        <template v-if="widget.type == 'cascader'">
          <el-cascader
            v-model="dataModel"
            :disabled="widget.options.disabled"
            :clearable="widget.options.clearable"
            :placeholder="widget.options.placeholder"
            :style="{width: widget.options.width}"
            :options="widget.options.remoteOptions"
          />
        </template>

        <template v-if="widget.type == 'text'">
          <span>{{ dataModel }}</span>
        </template>
      </el-form-item>
    </template>

  </div>
</template>

<script>
import { VueEditor } from 'vue2-editor'
import request from '@/util/request.js'

export default {
  components: {
    VueEditor
  },
  // eslint-disable-next-line
  props: ['widget', 'models', 'rules', 'remote'],
  data() {
    return {
      dataModel: this.models[this.widget.model]
    }
  },
  watch: {
    dataModel: {
      deep: true,
      handler(val) {
        this.models[this.widget.model] = val
        this.$emit('update:models', {
          ...this.models,
          [this.widget.model]: val
        })
        console.log('input-change', val, this.widget.model)
        this.$emit('input-change', val, this.widget.model)
      }
    },
    models: {
      deep: true,
      handler(val) {
        this.dataModel = val[this.widget.model]
      }
    }
  },
  created() {
    /**
     * 调用 api 获取数据
     */
    const options = this.widget.options
    // 有联动
    if (options.linkage && options.linkage.open) {
      // 向上派发联动监听
      this.$emit('add-linkage', options.linkage.model, this.widget.model)
    } else if (options.remote && options.remoteFunc) {
      this.fetchOptions()
    }

    /* // 调用 remoteFunc 请求数据
    const options = this.widget.options
    if (options.remote && this.remote[options.remoteFunc]) {
      this.remote[options.remoteFunc]((data) => {
        options.remoteOptions = data.map(item => {
          return {
            value: item[options.props.value],
            label: item[options.props.label],
            children: item[options.props.children]
          }
        })
      })
    } */

    /* if (this.widget.type === 'imgupload' && this.widget.options.isQiniu) {
      this.remote[this.widget.options.tokenFunc]((data) => {
        this.widget.options.token = data
      })
    } */
  },
  methods: {
    fetchOptions(params) {
      const options = this.widget.options
      request({
        url: `${options.remoteFunc}`,
        method: 'get',
        params
      }).then(res => {
        options.remoteOptions = res.data.map(item => {
          return {
            value: item[options.props.value],
            label: item[options.props.label],
            children: item[options.props.children]
          }
        })
      }).catch(e => {
        console.log(e)
      })
    },
    update(value) {
      const key = this.widget.options.linkage.key
      const params = JSON.parse(`{
        "${key}": "${value}"
      }`)
      this.fetchOptions(params)
    }
  }
}
</script>
