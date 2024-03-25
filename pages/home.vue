<template>
  <v-app>
    <v-app-bar color="blue-darken-2" density="compact" extension-height="60">
      <template v-slot:prepend>
        <v-img src="/同日图标.jpg" width="160px" alt="同日图标" />
        <div class="font-weight-bold text-h5 ml-1">工业互联网平台系统</div>
      </template>

      <template v-slot:append>
        <v-btn
          class="mr-3"
          icon="fa-solid fa-expand"
          @click="changeFullScreen()"
        ></v-btn>

        <v-menu open-on-hover open-delay="0" close-delay="100">
          <template v-slot:activator="{ props }">
            <v-avatar v-bind="props" class="mr-3">
              <v-img src="/头像.jpg" alt="头像" />
            </v-avatar>
          </template>

          <v-card class="pa-5" rounded="xl">
            <v-list width="180">
              <v-list-item
                rounded="lg"
                class="mb-3"
                prepend-avatar="/头像.jpg"
                :title="name || '默认用户'"
                subtitle="205451"
              >
              </v-list-item>

              <v-divider></v-divider>

              <v-list-item
                rounded="lg"
                @click="exit()"
                prepend-icon="fa-solid fa-right-from-bracket"
                subtitle="退出登录"
              >
              </v-list-item>
            </v-list>
          </v-card>
        </v-menu>
      </template>
    </v-app-bar>
    <v-navigation-drawer color="blue-darken-2" density="compact">
      <div v-for="(item_, index_) in items" :key="index_">
        <v-list v-for="(item, index) in item_.children" :key="index">
          <v-list-item
            v-show="item.show"
            :prepend-icon="item.icon"
            color="black"
            :title="item.name"
            :active="index === activeIndex"
            @click="changeComponent(item.path, index)"
          >
          </v-list-item>
        </v-list>
      </div>
    </v-navigation-drawer>
    <v-main>
      <component :is="currentComponent"></component>
    </v-main>
  </v-app>
</template>
<script setup lang="ts">
import { shallowRef, onMounted } from "vue";
import { useRouter } from "vue-router";
import PermissionDesign from "@/components/permission-design.vue";
import RoleCenter from "@/components/role-center.vue";
import UserCenter from "@/components/user-center.vue";
import Enrollment from "@/components/enrollment.vue";
import EnrollmentTwo from "@/components/enrollment-two.vue";
let activeIndex = ref(0);
const items = ref<any>();

onMounted(() => {
  items.value = useListToTree(useCookieJoin("menuList"));
  changeComponent(items.value[0].children[0].path, 0);
  console.log(items.value);
  console.log(items.value[0].children[0].path);
});
let currentComponent = shallowRef(null);
const componentsMap: Record<string, any> = {
  "/enrollment": Enrollment,
  "/enrollment-two": EnrollmentTwo,
  "/permission-design": PermissionDesign,
  "/role-center": RoleCenter,
  "/user-center": UserCenter,
};

function changeComponent(path: string, index: number) {
  activeIndex.value = index;
  const component = componentsMap[path];
  if (component) {
    currentComponent.value = component;
  } else {
    console.error("当前路径不存在");
  }
}

const router = useRouter();
const name = useCookie("name");
function exit() {
  router.push("/auth/login");
}
// 切换全屏模式
function changeFullScreen() {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen();
  } else {
    if (document.exitFullscreen) {
      document.exitFullscreen();
    }
  }
}
</script>
<style scoped>
.list-item.v-list-item--active {
  background-color: rgb(15, 15, 15); /* 激活状态下的背景颜色 */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2); /* 使用box-shadow添加阴影以模拟海拔 */
}
</style>
