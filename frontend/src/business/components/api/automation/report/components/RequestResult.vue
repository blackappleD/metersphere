<template>
  <div class="request-result">
    <p class="el-divider--horizontal"></p>
    <div @click="active">
      <el-row :gutter="10" type="flex" align="middle" class="info">
        <el-col :span="14" v-if="indexNumber!=undefined">
          <div class="method">
            <div class="el-step__icon is-text ms-api-col" v-if="indexNumber%2 ==0">
              <div class="el-step__icon-inner"> {{ indexNumber+1 }}</div>
            </div>
            <div class="el-step__icon is-text ms-api-col-create" v-else>
              <div class="el-step__icon-inner"> {{ indexNumber+1 }}</div>
            </div>
            {{ request.name }}
          </div>
        </el-col>

        <el-col :span="5">
          <el-tooltip effect="dark" :content="request.responseResult.responseCode" placement="bottom" :open-delay="800">
            <div style="color: #5daf34" v-if="request.success">{{ request.responseResult.responseCode }}</div>
            <div style="color: #FE6F71" v-else>{{ request.responseResult.responseCode }}</div>
          </el-tooltip>
        </el-col>
        <el-col :span="3">
          <span v-if="request.success">
            {{request.responseResult.responseTime}} ms
          </span>
          <span style="color: #FE6F71" v-else>
            {{request.responseResult.responseTime}} ms
          </span>
        </el-col>

        <el-col :span="2">
          <div>
            <el-tag size="mini" type="success" v-if="request.success">
              {{ $t('api_report.success') }}
            </el-tag>
            <el-tag size="mini" type="danger" v-else>
              {{ $t('api_report.fail') }}
            </el-tag>
          </div>
        </el-col>
      </el-row>
    </div>

    <el-collapse-transition>
      <div v-show="isActive" style="width: 99%">
        <ms-request-result-tail v-if="isActive" :request-type="requestType" :request="request"
                                :scenario-name="scenarioName"/>
      </div>
    </el-collapse-transition>
  </div>
</template>

<script>
  import MsRequestMetric from "./RequestMetric";
  import MsAssertionResults from "./AssertionResults";
  import MsRequestText from "./RequestText";
  import MsResponseText from "./ResponseText";
  import MsRequestResultTail from "./RequestResultTail";

  export default {
    name: "MsRequestResult",
    components: {MsResponseText, MsRequestText, MsAssertionResults, MsRequestMetric, MsRequestResultTail},
    props: {
      request: Object,
      scenarioName: String,
      indexNumber: Number,
    },
    data() {
      return {isActive: false, requestType: undefined,}
    },
    methods: {
      active() {
        this.isActive = !this.isActive;
        console.log(this.request)
        //this.$emit("requestResult", {request: this.request, scenarioName: this.scenarioName});
      }
    },
  }
</script>

<style scoped>
  .request-result {
    width: 100%;
    min-height: 40px;
    padding: 2px 0;
  }

  .request-result .info {
    margin-left: 20px;
    cursor: pointer;
  }

  .request-result .method {
    color: #1E90FF;
    font-size: 14px;
    font-weight: 500;
    line-height: 40px;
    padding-left: 5px;
  }

  .request-result .url {
    color: #7f7f7f;
    font-size: 12px;
    font-weight: 400;
    margin-top: 4px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    word-break: break-all;
  }

  .request-result .tab .el-tabs__header {
    margin: 0;
  }

  .request-result .text {
    height: 300px;
    overflow-y: auto;
  }

  .sub-result .info {
    background-color: #FFF;
  }

  .sub-result .method {
    border-left: 5px solid #1E90FF;
    padding-left: 20px;
  }

  .sub-result:last-child {
    border-bottom: 1px solid #EBEEF5;
  }

  .ms-api-col {
    background-color: #EFF0F0;
    border-color: #EFF0F0;
    margin-right: 10px;
    font-size: 12px;
    color: #64666A;
  }

  .ms-api-col-create {
    background-color: #EBF2F2;
    border-color: #008080;
    margin-right: 10px;
    font-size: 12px;
    color: #008080;
  }

  .el-divider--horizontal {
    margin: 2px 0;
    background: 0 0;
    border-top: 1px solid #e8eaec;
  }

</style>
