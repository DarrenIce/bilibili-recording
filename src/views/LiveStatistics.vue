<template>
    <el-dropdown>
        <span class="el-dropdown-link">
            Dropdown List<el-icon class="el-icon--right">
                <arrow-down />
            </el-icon>
        </span>
        <template #dropdown>
            <el-dropdown-menu>
                <el-dropdown-item v-for="brginfo in brgLst" :key="brginfo.timestamp" @click="fetchStatistics(brginfo)">
                    {{ date2string(new Date(brginfo.timestamp * 1000)) }} {{ brginfo.title }}
                </el-dropdown-item>

            </el-dropdown-menu>
        </template>
    </el-dropdown>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { useRoute } from 'vue-router'
import { ip, port } from "@/define/const";

interface brgInfo {
    title: string,
    file_name: string,
    timestamp: number,
}

export default defineComponent({
    setup() {
        const route = useRoute();
        return {
            route,
        }
    },
    data() {
        let brgLst: brgInfo[] = [];
        return {
            brgLst,
        }
    },
    mounted() {
        console.log(this.route.params.anchor)
        this.fetchBrgList()
    },
    methods: {
        fetchBrgList() {
            fetch(`${ip}:${port}/anchorlivebacklist/${this.route.params.anchor}`, {
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
                    let tmp: brgInfo[] = [];
                    for (let key in json) {
                        tmp.push(json[key])
                    }
                    this.brgLst = tmp;
                });
        },
        fetchStatistics(brginfo: brgInfo) {
            console.log(brginfo)
            fetch(`${ip}:${port}/livebackstatistics`, {
                method: "post",
                headers: {
                    "Access-Control-Allow-Origin": "*",
                    "Access-Control-Allow-Methods":
                        "POST, GET, OPTIONS, PUT, DELETE,UPDATE",
                    "Access-Control-Allow-Headers":
                        "Access-Control-Allow-Methods, Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers",
                    "Access-Control-Allow-Credentials": "true",
                    "Content-Type": "application/json; charset=utf-8",
                },
                body: JSON.stringify({
                    anchor_name: this.route.params.anchor,
                    brg_file_name: brginfo.file_name,
                }),
            })
                .then((res) => res.json())
                .then((json) => {
                    console.log(json);

                })
                .catch((err) => {
                    console.log(err);
                });
        },
        stillTwo(num: number) {
            return ("0" + num).substr(-2);
        },
        date2string(date: Date) {
            return `${date.getFullYear()}-${this.stillTwo(
                date.getMonth() + 1
            )}-${this.stillTwo(date.getDate())} ${this.stillTwo(
                date.getHours()
            )}:${this.stillTwo(date.getMinutes())}:${this.stillTwo(
                date.getSeconds()
            )}`;
        },
    }
})
</script>
