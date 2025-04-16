<script setup>
import { onMounted, ref } from 'vue';
import { onUnmounted } from 'vue';

// 读取白名单配置
const whitelist = ref([]);
const domainWhitelist = ref([]);
const isWhitelistEnabled = ref(false);

// 加载白名单配置
const loadWhitelist = async () => {
  try {
    const response = await fetch('/link-whitlist.json');
    const data = await response.json();
    isWhitelistEnabled.value = data.enable;
    if (data.links) {
      whitelist.value = data.links;
    }
    if (data.domain) {
      domainWhitelist.value = data.domain;
    }
  } catch (error) {
    console.error('Failed to load whitelist:', error);
  }
};

// 检查是否是外部链接
const isExternalLink = (href) => {
  try {
    if (!href || href.startsWith('#') || href.startsWith('javascript:')) return false;

    // 排除 external-link.html
    const parsedUrl = new URL(href, window.location.origin);
    if (parsedUrl.pathname === '/external-link.html') return false;

    const domain = (url) => new URL(url).hostname.replace('www.', '');
    return domain(window.location.href) !== domain(href);
  } catch {
    return false;
  }
};

// 检查是否在域名白名单中
const isDomainWhitelisted = (href) => {
  try {
    const domain = new URL(href).hostname.replace('www.', '');
    return domainWhitelist.value.some(domainUrl => {
      const whitelistDomain = new URL(domainUrl).hostname.replace('www.', '');
      return domain === whitelistDomain;
    });
  } catch {
    return false;
  }
};

// 处理外部链接点击事件
const handleExternalClick = (e) => {
  e.preventDefault();
  const href = e.currentTarget.href;
  console.log('Opening external link:', href); // 调试日志

  // 读取当前页面的显示模式
  const isDarkMode = document.documentElement.classList.contains('dark');
  const mode = isDarkMode ? 'dark' : 'normal';

  // 对链接进行 Base64 编码
  const encodedHref = btoa(href);

  // 打开新的标签页，跳转到 external-link.html，并将编码后的 URL 和模式作为参数传递
  window.open(`/external-link.html?url=${encodedHref}&mode=${mode}`, '_blank');
};

// 拦截所有外部链接
const interceptLinks = () => {
  document.querySelectorAll('a').forEach(link => {
    if (isExternalLink(link.href) && !link.classList.contains('no-intercept')) {
      // 检查是否在白名单中
      if (isWhitelistEnabled.value && (whitelist.value.includes(link.href) || isDomainWhitelisted(link.href))) {
        return; // 白名单中的链接不拦截
      }

      // 如果已经绑定过事件监听器，则跳过
      if (link.dataset.intercepted) return;

      // 标记链接已被拦截
      link.dataset.intercepted = true;

      // 绑定事件
      link.addEventListener('click', handleExternalClick);
    }
  });
};

onMounted(async () => {
  await loadWhitelist();
  interceptLinks();

  // 监听 DOM 变化，动态拦截新添加的链接
  let debounceTimer;
  const observer = new MutationObserver(() => {
    clearTimeout(debounceTimer);
    debounceTimer = setTimeout(() => interceptLinks(), 100); // 调整为 100ms
  });
  observer.observe(document.body, { childList: true, subtree: true });
});

onUnmounted(() => {
  document.querySelectorAll('a').forEach(link => {
    if (link.dataset.intercepted) {
      link.removeEventListener('click', handleExternalClick);
      delete link.dataset.intercepted; // 清除标记
    }
  });
});
</script>

<template>
  <!-- 这是一个无渲染组件 -->
</template>