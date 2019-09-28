<template>
  <div>
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app>
      <v-list dense>
        <template v-for="item in items">
          <v-layout v-if="item.heading" :key="item.heading" align-center>
            <v-flex xs6>
              <v-subheader v-if="item.heading">{{ item.heading }}</v-subheader>
            </v-flex>
            <v-flex xs6 class="text-center">
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-flex>
          </v-layout>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon
          >
            <template v-slot:activator>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>{{ item.text }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i" 
              @click="clickMenu(child.id)">
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>{{ child.text }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else :key="item.text" @click="clickMenu(item.id)">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark
      >
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="hidden-sm-and-down">手术麻醉工作站</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip right>
        <template v-slot:activator="{ on }">
          <v-btn icon to="/about" v-on="on">
            <v-icon>thumb_up_alt</v-icon>
          </v-btn>
        </template>
        <span>帮助手册</span>
      </v-tooltip>
      <v-btn icon to="/">
        <v-icon>notifications</v-icon>
      </v-btn>
      <v-btn icon large @click="selectSource">
        <v-avatar size="32px" item>
          <v-img :src="require('../assets/logo.svg')" alt="苜蓿草科技"></v-img>
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <!-- <v-container fluid fill-height>
      <v-layout justify-start>
        <v-img :src="require('../assets/main_bg.jpg')" />
      </v-layout>
    </v-container>-->
    <v-tooltip right>
      <template v-slot:activator="{ on }">
        <v-btn
          bottom
          color="pink"
          dark
          fab
          fixed
          right 
          @click="clickMenu('logout')"
          v-on="on"
          ><v-icon>add</v-icon>
        </v-btn>
      </template>
      <span>退出登录</span>
    </v-tooltip>
  </div>
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    dialog: false,
    drawer: null,
    items: [
      { icon: "contacts",
        text: "科室管理", 
        "icon-alt": "keyboard_arrow_down",
         model: false,
        children: [
          { text: "设置用户及权限", id: "op1",icon: "keyboard" },
          { text: "手术代码设置", id: "op2" ,icon: "keyboard"},
          { text: "手术等级设置", id: "op3",icon: "keyboard" },
          { text: "手术器械设置", id: "op4",icon: "keyboard" },
          { text: "医嘱组套设置", id: "op5",icon: "keyboard" },
          { text: "手术知情同意书模板设置", id: "op6",icon: "keyboard" }
        ]
      },
      { icon: "history",
        text: "手术排班", 
        "icon-alt": "keyboard_arrow_down",
         model: false,
        children: [
          { text: "病人检索", id: "op11",icon: "content_copy" },
          { text: "手术申请", id: "op12" ,icon: "content_copy"},
          { text: "手术安排", id: "op13",icon: "content_copy" },
          { text: "急诊手术", id: "op14",icon: "content_copy" },
          { text: "手术通知单", id: "op15",icon: "content_copy" }
        ]
      },
      { icon: "content_copy",
        text: "术前访视",
        "icon-alt": "keyboard_arrow_down",
        model: false,
        children: [
          { text: "会诊记录", id: "op21",icon: "content_copy" },
          { text: "麻醉方案", id: "op22" ,icon: "content_copy"},
          { text: "谈话记录", id: "op23",icon: "content_copy" }
        ]
      },
      { icon: "keyboard",
        text: "术中护理",
        "icon-alt": "keyboard_arrow_down",
        model: false,
        children: [
          { text: "手术记录", id: "op31",icon: "content_copy" },
          { text: "附加手术", id: "op32" ,icon: "content_copy"},
          { text: "器械清点", id: "op33",icon: "content_copy" },
          { text: "紫外线消毒记录", id: "op34",icon: "content_copy" },
          { text: "手指无菌培养", id: "op35",icon: "content_copy" },
          { text: "物品无菌检查", id: "op36",icon: "content_copy" },
          { text: "空气无菌检查", id: "op37",icon: "content_copy" },
          { text: "手术中空气无菌检查", id: "op38",icon: "content_copy" }
        ]
      },      
      {
        icon: "settings",
        "icon-alt": "keyboard_arrow_down",
        text: "术中麻醉",
        model: false,
        children: [
          { text: "麻醉前确认", id: "op41", icon: "content_copy" },
          { text: "手术安全核查", id: "op42",icon: "content_copy" },
          { text: "输血前指征", id: "op43",icon: "content_copy" },
          { text: "麻醉记录", id: "op44",icon: "content_copy" },
          { text: "麻醉医嘱", id: "op45",icon: "content_copy" }
        ]
      },
      {
        icon: "settings",
        "icon-alt": "keyboard_arrow_down",
        text: "麻醉恢复",
        model: false,
        children: [
          { text: "二次麻醉评估", id: "op51", icon: "content_copy" },
          { text: "麻醉药品确认单", id: "op52",icon: "content_copy" },
          { text: "不良事件", id: "op53",icon: "content_copy" }
        ]
      },
      {
        icon: "settings",
        "icon-alt": "keyboard_arrow_down",
        text: "术后镇痛",
        model: false,
        children: [
          { text: "术后镇痛评估", id: "op61", icon: "content_copy" },
          { text: "术后跟踪登记", id: "op62",icon: "content_copy" }
        ]
      },
      {
        icon: "settings",
        "icon-alt": "keyboard_arrow_down",
        text: "门诊手术",
        model: false,
        children: [
          { text: "内镜麻醉知情同意书", id: "op71", icon: "content_copy" },
          { text: "门诊全麻记录单", id: "op72",icon: "content_copy" },
          { text: "门诊麻醉术前访视", id: "op73",icon: "content_copy" },
          { text: "无痛人流知情同意书", id: "op74",icon: "content_copy" }
        ]
      },
      { icon: "help", text: "退出登录", id: "logout" }
    ]
  }),
  methods: {
    clickMenu(tstr) {
      console.log("点击=" + tstr);
      if (tstr === "logout") {
        localStorage.removeItem("user");
        this.$router.push({ path: "/login" });
      }
      switch (tstr)
      {
        case "out_reg":
          this.$router.push({ path: "/outreg" });
          break;
        case "out_cash":
          this.$router.push({ path: "/outcash" });
          break;
        case "out_chk":
          this.$router.push({ path: "/outchk" });
          break;
        case "out_receipt":
          this.$router.push({ path: "/outreceipt" });
          break;
        case "detail_reg":
          this.$router.push({ path: "/detailreg" });
          break;
        case "detail_cash":
          this.$router.push({ path: "/detailcash" });
          break;
        case "detail_chk":
          this.$router.push({ path: "/detailchk" });
          break;
        case "detail_undo":
          this.$router.push({ path: "/detailundo" });
          break;
        case "detail_op":
          this.$router.push({ path: "/detailop" });
          break;
        case "mg_dict":
          this.$router.push({ path: "/mgdict" });
          break;
        case "mg_analyse":
          this.$router.push({ path: "/mganalyse" });
          break;
        case "mg_invoice":
          this.$router.push({ path: "/mginvoice" });
          break;
        default:
          localStorage.removeItem("user");
          this.$router.push({ path: "/login" });
      }
    },

    selectSource() {
      window.location.href = "http://www.cloveropen.com";
    }
  }
};
</script>
