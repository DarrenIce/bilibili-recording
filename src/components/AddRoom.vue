<template>
    <div></div>
    <!-- <el-drawer v-model="addFormVisible" title="添加房间" size='45%' direction="ltr">
                <el-form :model="addForm">  
                    <el-form-item label="房间ID" label-width="120px">
                        <el-input v-model="addForm.RoomID" autocomplete="off"></el-input>
                    </el-form-item>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.RecordMode" active-color="#13ce66" inactive-color="#ff4949"
                                    active-text="24小时录制" inactive-text="仅在设定时间录制"
                                    @change="changeAddRecordTimeText(addForm.RecordMode)" />
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.DivideByTitle" active-color="#13ce66"
                                    inactive-color="#ff4949" active-text="当天录像按照标题分P" inactive-text="当天录像不分P" />
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="录制开始时间" label-width="120px">
                                <el-time-picker v-model="addForm.StartTime" :disabled="disableAddRecord">
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="录制结束时间" label-width="120px">
                                <el-time-picker v-model="addForm.EndTime" :disabled="disableAddRecord">
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.AutoRecord" active-color="#13ce66" inactive-color="#ff4949"
                                    active-text="自动录制" inactive-text="仅监控" />
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.AutoUpload" disabled active-color="#13ce66"
                                    inactive-color="#ff4949" active-text="自动上传" inactive-text="禁止上传" />
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.NeedM4a" active-color="#13ce66" inactive-color="#ff4949"
                                    active-text="转码生成视频和音频" inactive-text="仅生成视频" />
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.Mp4Compress" active-color="#13ce66"
                                    inactive-color="#ff4949" active-text="视频质量压缩" inactive-text="不压缩视频" />
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.CleanUpRegular" active-color="#13ce66"
                                    inactive-color="#ff4949" active-text="定期清理录制内容" inactive-text="不清理生成的文件" />
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="文件保留时间" label-width="120px">
                                <el-input v-model="addForm.SaveDuration" autocomplete="off"
                                    :disabled="!addForm.CleanUpRegular"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item>
                                <el-switch v-model="addForm.AreaLock" active-color="#13ce66" inactive-color="#ff4949"
                                    active-text="只录制指定分区下的直播" inactive-text="不限制直播分区" />
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="指定的分区名" label-width="120px">
                                <el-input v-model="addForm.AreaLimit" autocomplete="off" :disabled="!addForm.AreaLock">
                                </el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
                <div class="drawer-footer">
                    <el-button-group>
                        <el-button icon="el-icon-circle-close" @click="addFormVisible = false">取消</el-button>
                        <el-button icon="el-icon-circle-check" type="primary" @click="addOnSubmit">确定</el-button>
                    </el-button-group>
                </div>
            </el-drawer> -->
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export const AddRoomComponent = defineComponent({
    props: {
        isShowAddRoom: {
            type: Boolean,
            required: true,
        }
    },

    setup() {
        
    },
})
</script>
