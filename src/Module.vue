<template>
  <div class="module-container">
    <div id="modulo-basa">MÓDULO SERÁ MONTADO AQUI</div>
    <div class="foobar">
      <AtButton @click="mountGleba">Montar gleba</AtButton>
      <AtButton @click="mountAnalyze">Montar análises</AtButton>
      <AtButton @click="unmount">Desmontar</AtButton>
    </div>
  </div>
</template>

<script lang="ts">
import getModule from '@agrotools1/at-mf-utils/dist/build'
import { AtButton } from '@agrotools1/at-components'
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
  name: 'AtModule',
  components: {
    AtButton,
  },
  setup() {
    const module = ref<{
      mountWithGleba: (selector: string) => void
      mountWithAnalyzeResults: (selector: string) => void
      unmount: () => void
    } | null>(null)

    const mountGleba = async () => {
      module.value?.mountWithGleba('#modulo-basa')
    }

    const mountAnalyze = async () => {
      module.value?.mountWithAnalyzeResults('#modulo-basa')
    }

    const unmount = async () => {
      module.value?.unmount()
    }

    onMounted(async () => {
      const imports = (await getModule(
        'basacadastro',
        'https://basa-modules-test.agrotools.com.br/cadastro/remoteEntry.js',
      )) as unknown as typeof module.value

      module.value = imports
    })

    return { mountGleba, mountAnalyze, unmount }
  },
})
</script>

<style lang="scss">
#q-app {
  height: 100%;
  font-family: 'Nunito';
}

.module-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-flow: column nowrap;
  gap: 10px;
  align-items: center;
}
body {
  padding: 0;
  margin: 0;
  height: 100%;
  width: 100%;
}
html {
  height: 100%;
}

.foobar {
  display: flex;
  gap: 10px;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin: 10px 2px;
}
</style>
