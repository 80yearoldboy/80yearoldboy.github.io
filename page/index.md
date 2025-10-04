---
title: 正在重定向
---
<meta name="msvalidate.01" content="BFEB5BF33AB4DC1743274D9CE1DCCCFD" />
<script setup>
import { onMounted } from "vue"
import { useRouter } from "vitepress"
    
const router = useRouter();

onMounted(() => router.go("/"));
</script>
