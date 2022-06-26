<template>
  <el-row :gutter="10">
    <el-col :span="12">
      <el-input v-model="searchTitle" placeholder="Search Title" clearable>
        <template #append>
          <el-icon>
            <Search />
          </el-icon>
        </template>
      </el-input>
    </el-col>
    <el-col :span="12">
      <el-input v-model="searchAnchor" placeholder="Search Anchor" clearable>
        <template #append>
          <el-icon>
            <Search />
          </el-icon>
        </template>
      </el-input>
    </el-col>
  </el-row>
  <el-row :gutter="10">
    <el-col :span="2" v-for="area in areas" :key="area">
      <el-tag @click="onClickArea(area)">
        {{ area }}
      </el-tag>
    </el-col>
  </el-row>
  <el-row :gutter="10" v-if="showSubArea">
    <el-col :span="2" v-for="area in subAreas" :key="area">
      <el-tag @click="onClickSubArea(area)">
        {{ area }}
      </el-tag>
    </el-col>
  </el-row>
  <el-row :gutter="5">
    <el-col
      :xs="16"
      :sm="10"
      :md="8"
      :lg="5"
      :xl="4"
      v-for="item in showrooms"
      :key="item.RoomID"
    >
      <el-card shadow="always" style="--el-card-padding: 10px">
        <el-space direction="vertical" alignment="start">
          <!-- <el-link :href="roomID2Url(item.RoomID)" :underline="false"> -->
          <img
            :src="item.LiveCover"
            style="width: 256px; height: 144px"
            class="image"
            lazy
          />
          <!-- </el-link> -->
          <i class="el-icon-user" style="font-size: small">{{
            item.Popularity
          }}</i>
          <div>{{ item.Title.slice(0, 15) }}</div>
          <!-- <el-link :href="roomID2Url(item.RoomID)">{{ item.Title }}</el-link> -->
          <div style="color: #909399">
            <el-avatar size="small" :src="item.UserCover"></el-avatar>
            <span>{{ item.Uname }}</span>
            <!-- <el-link :href="uID2url(item.UID)">{{ item.Uname }}</el-link> -->
            <!-- <el-button
              style="margin-left: 10px"
              v-if="judgeExists(item.RoomID)"
              size="small"
              icon="el-icon-plus"
              @click="clickMonitorRoom(item.RoomID)"
              circle
            ></el-button>
            <el-button
              size="small"
              icon="el-icon-remove"
              @click="blockRoom(item.RoomID)"
              circle
            ></el-button> -->
          </div>
        </el-space>
      </el-card>
    </el-col>
  </el-row>
  <el-pagination
    layout="prev, pager, next"
    :page-count="pages"
    :current-page="pageIndex"
    @update:current-page="onCurrentChange"
  />
</template>

<script lang="ts">
import { MonitorRoom } from "@/define/interface";
import { defineComponent, ref } from "vue";
import { ip, port } from "@/define/const";

export default defineComponent({
  setup() {},
  data() {
    const searchTitle = ref("");
    const searchAnchor = ref("");
    let areas: string[] = [];
    let subAreas: string[] = [];
    let allrooms: MonitorRoom[] = [];
    let arearooms: MonitorRoom[] = [];
    let showrooms: MonitorRoom[] = [];
    let showSubArea: boolean = false;
    const perPageNum: number = 30;
    let pages: number = 0;
    let pageIndex: number = 1;
    return {
      searchTitle,
      searchAnchor,
      areas,
      subAreas,
      allrooms,
      arearooms,
      showrooms,
      showSubArea,
      perPageNum,
      pages,
      pageIndex,
    };
  },
  mounted() {
    this.getAreaInfo(true);
    setInterval(this.getAreaInfo, 1000);
  },
  methods: {
    getAreaInfo(give: boolean = false) {
      fetch(`${ip}:${port}/areainfos`, {
        method: "get",
        headers: {
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Methods":
            "POST, GET, OPTIONS, PUT, DELETE,UPDATE",
          "Access-Control-Allow-Headers":
            "Access-Control-Allow-Methods, Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers",
          "Access-Control-Allow-Credentials": "true",
          "Content-Type": "application/json; charset=utf-8",
        },
      })
        .then((res) => {
          if (!res.ok) {
            console.log("failed");
          }
          return res.json();
        })
        .then((json) => {
          let data: MonitorRoom[] = [];
          let tmpareas: string[] = [];
          for (let key in json) {
            data.push(json[key]);
            if (tmpareas.indexOf(json[key]["ParentName"]) === -1) {
              tmpareas.push(json[key]["ParentName"]);
            }
          }
          this.allrooms = data;
          this.areas = tmpareas;
          if (give) {
            this.arearooms = data;
            this.showrooms = this.arearooms.slice(0, this.perPageNum);
            this.pages = ((this.arearooms.length / this.perPageNum) | 0) + 1;
            this.pageIndex = 1;
          }
          // console.log(this.roomTable);
        });
    },
    onClickArea(area: string) {
      let tmpsubareas: string[] = [];
      let tmpdata: MonitorRoom[] = [];
      for (let key in this.allrooms) {
        if (this.allrooms[key]["ParentName"] === area) {
          tmpdata.push(this.allrooms[key]);
          if (tmpsubareas.indexOf(this.allrooms[key]["AreaName"]) === -1) {
            tmpsubareas.push(this.allrooms[key]["AreaName"]);
            this.showSubArea = true;
          }
        }
      }
      this.arearooms = tmpdata;
      this.subAreas = tmpsubareas;
      this.showrooms = this.arearooms.slice(0, this.perPageNum);
      this.pages = ((this.arearooms.length / this.perPageNum) | 0) + 1;
      this.pageIndex = 1;
    },
    onClickSubArea(subarea: string) {
      let tmpdata: MonitorRoom[] = [];
      for (let key in this.allrooms) {
        if (this.allrooms[key]["AreaName"] === subarea) {
          tmpdata.push(this.allrooms[key]);
        }
      }
      this.arearooms = tmpdata;
      this.showrooms = this.arearooms.slice(0, this.perPageNum);
      this.pages = ((this.arearooms.length / this.perPageNum) | 0) + 1;
      this.pageIndex = 1;
    },
    onCurrentChange(index: number) {
      this.showrooms = this.arearooms.slice(
        (index - 1) * this.perPageNum,
        index * this.perPageNum
      );
      this.pageIndex = index;
    },
  },
});
</script>

<style>
.el-row {
  margin-bottom: 10px;
}
</style>
