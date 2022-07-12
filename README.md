---
home: true
heroText: 
tagline: 
---

<main aria-labelledby="main-title">
  <header class="hero">
    <img src="/assets/LL-Logo.png" alt="hero"> 
    <h1 id="main-title">
      LiteLoaderBDS
    </h1>
    <p class="description">
      Легкий &amp; мульти-язычный загрузчик плагинов для Minecraft Bedrock Dedicated Server
    </p>
    <p class="action">
      <a href="/ru_RU/" class="nav-link action-button documentation-button">
        Документация
      </a>
      <a href="https://github.com/LiteLDev/LiteLoaderBDS/releases/latest" class="nav-link action-button download-button">
        Скачать
      </a>
    </p>
    <p class="version">Версия LiteLoaderBDS: <b>{{ LiteLoaderVersion }}</b> <span>•</span>Версия BDS: <b>{{ BDSVersion }}</b></p>
  </header>
  <div class="features">
    <div class="feature" v-for="feature in LiteLoaderFeatures" :id="feature.title">
      <h2>{{ feature.title }}</h2>
      <p>{{ feature.subtitle }}</p>
    </div>
  </div>
</main>

::: slot footer
AGPLv3 with extra restrictions & exceptions. <br /> Copyright © 2020 — Present LiteLDev.
:::

<style>
  .version {
    opacity: 0.7;
    margin: 0 3px;
  }

  .version span {
    opacity: 0.3;
  }
</style>

<script>
export default {
  data() {
    return {
      LiteLoaderFeatures: [
        {
          title: "Открытый",
          subtitle: "Полный доступ ко всем функциям и классам"
        },
        {
          title: "Эффективный",
          subtitle: "Автоматически сгенерированные заголовки C++ "
        },
        {
          title: "Расширяемый",
          subtitle: "Мощное апи"
        }
      ],
      LiteLoaderVersion: "2.X.X",
      BDSVersion: "1.XX.X",
    }
  },
  mounted() {
    fetch("https://api.github.com/repos/LiteLDev/LiteLoaderBDS/tags")
      .then(response => response.json())
      .then(information => {
        this.LiteLoaderVersion = information[0].name;
      });
    
    fetch('https://raw.githubusercontent.com/LiteLDev/LiteLoaderBDS/main/Scripts/LINK.txt')
      .then(response => response.text())
      .then(bdsurl => {
        let BdsVersion = bdsurl.split('/')
        BdsVersion = BdsVersion[BdsVersion.length - 1].split('-')
        BdsVersion = BdsVersion[BdsVersion.length - 1].replace('.zip', '')

        this.BDSVersion = BdsVersion;
      })
  }
}
</script>