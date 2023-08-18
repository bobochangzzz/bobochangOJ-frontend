<template>
  <a-dropdown trigger="hover">
    <a-avatar shape="circle">
      <template
        v-if="loginUser && loginUser.userRole as string !== ACCESS_ENUM.NOT_LOGIN"
      >
        <template v-if="loginUser.userAvatar">
          <img alt="avatar" :src="loginUser.userAvatar" />
        </template>
        <template v-else>
          <a-avatar>
            <IconUser />
          </a-avatar>
        </template>
      </template>
      <template v-else>
        <a-avatar>未登录</a-avatar>
      </template>
    </a-avatar>
    <template #content>
      <template
        v-if="loginUser && loginUser.userRole as string !==
      ACCESS_ENUM.NOT_LOGIN"
      >
        <a-doption>
          <template #icon>
            <icon-idcard />
          </template>
          <template #default>
            <a-anchor-link>个人信息</a-anchor-link>
          </template>
        </a-doption>
        <a-doption>
          <template #icon>
            <icon-poweroff />
          </template>
          <template #default>
            <a-anchor-link @click="logout">退出登录</a-anchor-link>
          </template>
        </a-doption>
      </template>
      <template v-if="loginUser.userName === '未登录'">
        <a-doption>
          <template #icon>
            <icon-user />
          </template>
          <template #default>
            <a-anchor-link href="/user/login">登录</a-anchor-link>
          </template>
        </a-doption>
      </template>
    </template>
  </a-dropdown>
</template>
<script setup lang="ts">
import ACCESS_ENUM from "@/access/accessEnum";
import { LoginUserVO, UserControllerService } from "../../../../generated";
import { useStore } from "vuex";
import { computed } from "vue";

const store = useStore();

const loginUser: LoginUserVO = computed(
  () => store.state.user?.loginUser
) as LoginUserVO;

const logout = () => {
  UserControllerService.userLogoutUsingPost();
  location.reload();
};
</script>
