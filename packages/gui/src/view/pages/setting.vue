<template>
  <ds-container>
    <template slot="header">
      设置
      <span>
      </span>
    </template>

    <div v-if="config">
      <a-form-item label="开机自启" :label-col="labelCol" :wrapper-col="wrapperCol">
        <a-checkbox v-model="config.app.autoStart.enabled" @change="onAutoStartChange">
          本应用开机自启
        </a-checkbox>
        <div class="form-help">
          windows下建议开启开机自启。<a @click="openExternal('https://gitee.com/docmirror/dev-sidecar/blob/master/doc/recover.md')">更多说明参考</a>
        </div>
      </a-form-item>
      <a-form-item label="隐藏Dock图标" :label-col="labelCol" :wrapper-col="wrapperCol">
        <a-checkbox v-model="config.app.dock.hideWhenWinClose" >
          关闭窗口时隐藏Dock图标(仅限Mac)
        </a-checkbox>
        <div class="form-help">
          修改后需要重启应用
        </div>
      </a-form-item>
      <a-form-item label="远程配置" :label-col="labelCol" :wrapper-col="wrapperCol">
        <a-checkbox v-model="config.app.remoteConfig.enabled" @change="onRemoteConfigEnabledChange">
          启用远程配置
        </a-checkbox>
        <div class="form-help">
          应用启动时会向下面的地址请求配置补丁，无需发布升级包即可获得最新优化后的github访问体验。
          <br/>如果您觉得远程更新配置有安全风险，请关闭此功能。
        </div>
      </a-form-item>
      <a-form-item label="远程配置地址" :label-col="labelCol" :wrapper-col="wrapperCol">
        <a-input v-model="config.app.remoteConfig.url"></a-input>
      </a-form-item>
    </div>
    <template slot="footer">
      <div class="footer-bar">
        <a-button class="md-mr-10" icon="sync"   @click="resetDefault()">恢复默认</a-button>
        <a-button :loading="applyLoading" icon="check" type="primary" @click="apply()">应用</a-button>
      </div>
    </template>
  </ds-container>

</template>

<script>
import Plugin from '../mixins/plugin'
export default {
  name: 'Setting',
  mixins: [Plugin],
  data () {
    return {
      key: 'app'
    }
  },
  created () {

  },
  mounted () {
  },
  methods: {
    openExternal (url) {
      this.$api.ipc.openExternal(url)
    },
    onAutoStartChange () {
      this.$api.autoStart.enabled(this.config.app.autoStart.enabled)
      this.saveConfig()
    },
    onRemoteConfigEnabledChange () {
      this.saveConfig()
      this.$message.info('请重启加速服务')
    }
  }
}
</script>
<style lang="sass">
</style>
