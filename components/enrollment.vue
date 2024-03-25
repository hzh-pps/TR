<script setup lang="ts">
// 搜索引擎优化
useSeoMeta({
  // 该页面的标题
  title: "新增资料",
  // 社交媒体分享该页面时显示的标题
  ogTitle: "新增资料",
  // 该页面的描述
  description: "同日数字化工厂系统，新增资料",
  // 社交媒体分享该页面时显示的描述
  ogDescription: "同日数字化工厂系统，新增资料",
  // 社交媒体分享该页面时显示的图片
  ogImage: "/同日图标.png",
});
// 获取消息条对象
const { snackbarShow, snackbarColor, snackbarText, setSnackbar } =
  useSnackbar();
let user = ref<any>({
  company_name: "同日云联", //单位名称
  address: "昆山市花桥镇花安路", //单位住址
  registration_date: "2022-01-01", //注册时间
  office_phone: "0512-12345678", //办公电话
  legal_representative_name: "张三", //法定代表人姓名
  legal_representative_phone: "17839753802", //法定代表人电话
  business_scope: "软件开发", //经营范围
  represents_association_name: "李四", //事务代表人
  contact_phone: "15796371403", //事务代表人电话
  position: "经理", //职务
  political_status: "党员", //政治面貌
  id_number: "4111111111111111111111", //身份证
  contact_person_phone: "15802148508", //对接人电话
  contact_person_name: "王五", //对接人
  contactPhone: "0512-12345678", //联系电话
  email: "123456@qq.com", //联系邮箱
  contact_person_email: "212223@qq.com", //对接人邮箱
  membership_application: "",
  attachment_1: "",
  attachment_2: "",
});

let selectedFile = ref<File | null>(null);
let selectedFileName = ref<any>(null);
// 处理文件选择事件
function handleFileChange(event: Event) {
  // 获取input元素

  const input = event.target as HTMLInputElement;
  // 确保文件已被选择
  if (input.files && input.files.length > 0) {
    // 获取第一个文件
    selectedFile.value = input.files[0];
    console.log(selectedFile.value); // 在这里可以进一步处理文件，比如读取文件内容
  } else {
    selectedFile.value = null;
    console.log("没有文件");
  }
}

let url = ref<any[]>([]);
// 获取 token
const token = useCookie("token");
async function upLoadFile() {
  if (selectedFile.value) {
    try {
      const formData = new FormData();
      formData.append("file", selectedFile.value);
      formData.append("file_name", selectedFile.value.name);
      formData.append("user_name", "张三"),
        formData.append("is_encrypted", "0");
      // 其他formData.append操作
      const data: any = await useHttp("/File/W01AddOnePdf", "post", formData);
      if (data.code === 200) {
        url.value.push(data.data.fileurl);
        setSnackbar("green", selectedFile.value.name + "上传成功");
        selectedFileName.value = null;
      }
    } catch (error) {
      console.error("上传文件时发生错误：", error);
    }
  } else {
    console.log("没有选择文件");
  }
}
// 保存
async function savePerson() {
  user.value.attachment_1 = url.value[0];
  user.value.attachment_2 = url.value[1];
  if (user.value.company_name === "") {
    return setSnackbar("black", "公司的名称不能为空");
  }
  if (user.value.office_phone === "") {
    return setSnackbar("black", "办公电话不能为空");
  }
  if (user.value.registration_date === "") {
    return setSnackbar("black", "注册时间不能为空");
  }
  if (user.value.legal_representative_name === "") {
    return setSnackbar("black", "法定代表人姓名不能为空");
  }
  if (user.value.legal_representative_phone === "") {
    return setSnackbar("black", "法定代表人电话不能为空");
  }
  if (user.value.business_scope === "") {
    return setSnackbar("black", "经营范围不能为空");
  }
  if (user.value.represents_association_name === "") {
    return setSnackbar("black", "事务代表人不能为空");
  }
  if (user.value.contact_phone === "") {
    return setSnackbar("black", "事务代表人电话不能为空");
  }
  if (user.value.position === "") {
    return setSnackbar("black", "职务不能为空");
  }
  if (user.value.political_status === "") {
    return setSnackbar("black", "政治面貌不能为空");
  }
  if (user.value.id_number === "") {
    return setSnackbar("black", "身份证不能为空");
  }
  if (user.value.contact_person_name === "") {
    return setSnackbar("black", "对接人不能为空");
  }
  if (user.value.contact_person_phone === "") {
    return setSnackbar("black", "对接人电话不能为空");
  }

  const data: any = await useHttp(
    "/Association/AddMemberInformation",
    "post",
    user.value
  );
  if (data.code === 200) {
    setSnackbar("green", "保存成功");
    user.value = null;
    url.value = [];
  }
}
</script>
<template>
  <v-row class="ma-1">
    <v-col cols="8">
      <v-file-input
        show-size
        label="上传文件"
        v-model="selectedFileName"
        @change="handleFileChange"
      ></v-file-input>
    </v-col>
    <v-col cols="4">
      <v-btn
        v-show="url.length !== 2"
        color="blue-darken-2"
        class="mr-2 mt-2"
        size="default"
        @click="upLoadFile"
        >上传文件</v-btn
      >
    </v-col>
    <v-col cols="12" class="d-flex align-center justify-center">
      <div class="d-flex flex-column" style="width: 794px; height: 1123px">
        <div class="d-flex justify-center">
          <h3 class="text-center">单位会员资料维护</h3>
        </div>
        <div class="mt-6">
          <h4>昆山市工业互联网产业协会：</h4>
        </div>
        <div class="mt-2">
          <table border="1" style="border-collapse: collapse">
            <tbody>
              <!-- 第一行 -->
              <tr>
                <td
                  style="
                    height: 80px;
                    width: 150px;
                    text-align: center; /* 使文字居中 */
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  单位名称（全称）
                </td>
                <td style="height: 80px; width: 300px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.company_name"
                    type="text"
                  />
                </td>
                <td
                  style="
                    width: 100px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  单位住址
                </td>
                <td style="height: 80px; width: 250px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.address"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第二行 -->
              <tr>
                <td
                  style="
                    text-align: center;
                    height: 40px;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  注册时间
                </td>
                <td style="text-align: center; height: 40px">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.registration_date"
                    type="text"
                  />
                </td>
                <td
                  style="text-align: center; font-weight: bold; /* 文字加粗 */"
                >
                  办公电话
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.office_phone"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第三行 -->
              <tr>
                <td
                  style="
                    height: 60px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  法定代表人姓名
                </td>
                <td style="height: 60px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.legal_representative_name"
                    type="text"
                  />
                </td>
                <td
                  style="
                    height: 60px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  联系电话
                </td>
                <td style="height: 60px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.legal_representative_phone"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第四行 -->
              <tr>
                <td
                  style="
                    height: 300px;
                    max-height: 350px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  经营范围
                </td>
                <td colspan="3" style="height: 300px; padding: 0">
                  <textarea
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      text-indent: 2em;
                      box-sizing: border-box;
                      padding: 10px;
                      /* 使文字居中 */
                      resize: none; /* 禁止调整大小 */
                    "
                    v-model="user.business_scope"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第五行 -->
              <tr>
                <td
                  colspan="2"
                  style="
                    height: 80px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  单位派出参与协会事务代表姓名（须有授权委托书，注明授权对象基本信息、授权范围）
                </td>
                <td colspan="2" style="height: 80px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.represents_association_name"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第六行 -->
              <tr>
                <td
                  style="
                    text-align: center;
                    height: 40px;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  职务
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.position"
                    type="text"
                  />
                </td>
                <td
                  style="
                    text-align: center;
                    height: 40px;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  政治面貌
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.political_status"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第七行 -->
              <tr>
                <td
                  style="
                    height: 40px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  事务代表人电话
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.contact_phone"
                    type="text"
                  />
                </td>
                <td
                  style="
                    height: 40px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  身份证号码
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.id_number"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第八行 -->
              <tr>
                <td
                  style="
                    height: 40px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  事务代表人邮箱
                </td>
                <td colspan="3" style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.email"
                    type="text"
                  />
                </td>
              </tr>
              <!-- 第九行 -->
              <tr>
                <td
                  colspan="2"
                  style="
                    height: 40px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  日常对接人
                </td>
                <td colspan="2" style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.contact_person_name"
                    type="text"
                  />
                </td>
              </tr>
              <tr>
                <td
                  style="
                    height: 40px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  对接人电话
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.contact_person_phone"
                    type="text"
                  />
                </td>
                <td
                  style="
                    height: 40px;
                    text-align: center;
                    font-weight: bold; /* 文字加粗 */
                  "
                >
                  对接人邮箱
                </td>
                <td style="height: 40px; padding: 0">
                  <input
                    style="
                      width: 100%; /* 使input占满整个td */
                      height: 100%;
                      box-sizing: border-box;
                      text-align: center; /* 使文字居中 */
                    "
                    v-model="user.contact_person_email"
                    type="text"
                  />
                </td>
              </tr>
            </tbody>
          </table>
          <div class="mt-4 d-flex justify-end">
            <v-btn
              v-show="url.length === 2"
              color="blue-darken-2"
              class="mr-2 mt-2"
              size="default"
              @click="savePerson"
            >
              保存信息
            </v-btn>
          </div>
        </div>
      </div>
    </v-col>
    <v-snackbar location="top" v-model="snackbarShow" :color="snackbarColor">
      {{ snackbarText }}
      <template v-slot:actions>
        <v-btn variant="tonal" @click="snackbarShow = false">关闭</v-btn>
      </template>
    </v-snackbar>
  </v-row>
</template>
