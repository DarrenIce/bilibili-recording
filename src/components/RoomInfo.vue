<template>
  <el-drawer v-model="isShowRoomInfo" title="房间信息" size="45%">
    <el-descriptions
      class="margin-top"
      :column="2"
      :data="roomInfo"
      border
    >
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-user"></i>
          房间ID
        </template>
        <el-link
          :type="status2Type(roomInfo.LiveStatus)"
          :href="roomID2URL(roomInfo)"
        >
          {{ roomInfo.RoomID }}
        </el-link>
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-mobile-phone"></i>
          主播
        </template>
        {{ roomInfo.Uname }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-location-outline"></i>
          分区
        </template>
        {{ roomInfo.AreaName }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-tickets"></i>
          直播标题
        </template>
        {{ roomInfo.Title }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          直播状态
        </template>
        <el-tag size="small" :type="status2Type(roomInfo.LiveStatus)"
          >{{ status2Name(roomInfo.LiveStatus) }}
        </el-tag>
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          开播时间
        </template>
        {{ roomInfo.LiveStartTime }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          开播时长
        </template>
        {{ roomInfo.LiveTime }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          录制时长
        </template>
        {{ roomInfo.RecordTime }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          转码时长
        </template>
        {{ roomInfo.DecodeTime }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          上传时长
        </template>
        {{ roomInfo.UploadTime }}
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          当前状态
        </template>
        <el-tag size="small" :type="state2Type(roomInfo.State)"
          >{{ state2Name(roomInfo.State) }}
        </el-tag>
      </el-descriptions-item>
      <el-descriptions-item>
        <template #label>
          <i class="el-icon-office-building"></i>
          占用空间
        </template>
        null
      </el-descriptions-item>
    </el-descriptions>
    <el-divider>
      <span>编辑房间信息</span>
    </el-divider>
    <el-form :model="roomInfo">
      <el-form-item label="房间ID" label-width="120px">
        <el-input v-model="roomInfo.RoomID" autocomplete="off"></el-input>
      </el-form-item>
      <el-row>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.RecordMode"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="24小时录制"
              inactive-text="仅在设定时间录制"
            />
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.DivideByTitle"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="当天录像按照标题分P"
              inactive-text="当天录像不分P"
            />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="录制开始时间" label-width="120px">
            <el-time-picker
              v-model="roomInfo.StartTime"
              :disabled="roomInfo.RecordMode"
            ></el-time-picker>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="录制结束时间" label-width="120px">
            <el-time-picker
              v-model="roomInfo.EndTime"
              :disabled="roomInfo.RecordMode"
            ></el-time-picker>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.AutoRecord"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="自动录制"
              inactive-text="仅监控"
            />
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.AutoUpload"
              disabled
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="自动上传"
              inactive-text="禁止上传"
            />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.NeedM4a"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="转码生成视频和音频"
              inactive-text="仅生成视频"
            />
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.Mp4Compress"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="视频质量压缩"
              inactive-text="不压缩视频"
            />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.CleanUpRegular"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="定期清理录制内容"
              inactive-text="不清理生成的文件"
            />
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="文件保留时间" label-width="120px">
            <el-input
              v-model="roomInfo.SaveDuration"
              autocomplete="off"
              :disabled="!roomInfo.CleanUpRegular"
            ></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item>
            <el-switch
              v-model="roomInfo.AreaLock"
              active-color="#13ce66"
              inactive-color="#ff4949"
              active-text="只录制指定分区下的直播"
              inactive-text="不限制直播分区"
            />
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="指定的分区名" label-width="120px">
            <el-input
              v-model="roomInfo.AreaLimit"
              autocomplete="off"
              :disabled="!roomInfo.AreaLock"
            >
            </el-input>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
    <template #footer>
      <el-button-group>
        <el-button
          type="danger"
          icon="el-icon-delete"
          @click="onDelete(roomInfo)"
          >删除
        </el-button>
        <el-button icon="el-icon-circle-close" @click="onCancel()"
          >取消</el-button
        >
        <el-button icon="el-icon-circle-check" type="primary" @click="onSubmit(roomInfo)"
          >确定</el-button
        >
        <el-button icon="el-icon-refresh" type="primary" @click="onDecode(roomInfo)"
          >立即转码</el-button
        >
      </el-button-group>
    </template>
  </el-drawer>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { RoomInfo } from "@/define/interface";
import {
  status2Type,
  status2Name,
  state2Type,
  state2Name,
} from "@/define/methods";

export default defineComponent({
  props: {
    isShow: Boolean,
    room: Object as PropType<RoomInfo>,
  },
  emits: [
    'update:isShow',
    'update:room',
  ],
  setup(props) {
    console.log(props)
    return {
      isShowRoomInfo: props.isShow,
      roomInfo: props.room,
      status2Name,
      status2Type,
      state2Type,
      state2Name,
    };
  },
  methods: {
    roomID2URL(item: RoomInfo) {
      if (item.Platform == "bilibili") {
        return `https://live.bilibili.com/${item.RoomID}`;
      } else if (item.Platform == "douyin") {
        return `https://live.douyin.com/${item.RoomID}`;
      } else if (item.Platform == "huya") {
        return `https://www.huya.com/${item.RoomID}`;
      }
      return "";
    },
    onDelete(item: RoomInfo) {
      console.log(item);
    },
    onSubmit(item: RoomInfo) {
      console.log(item);
    },
    onDecode(item: RoomInfo) {
      console.log(item);
    },
    onCancel() {
      this.isShowRoomInfo = false;
      this.$emit('update:isShow', this.isShowRoomInfo);
    }
  },
});
</script>
