<template>
  <div transition="duration-500" :min-h="inOrder ? '20 sm:26' : '16 sm:22'" 
  :min-w="inOrder ? '28 sm:48' : '24 sm:40'" flex flex-col items-center text-start
    justify="center" b="dashed 4 rounded-2" 
    :style="{
      borderColor: lastCard?.color || 'gray'
    }" 
    box="border" p="2 sm:3" m-2>
    <div w="100%" flex justify="start">
      <img display="none sm:block" transition="duration-500" :w="inOrder ? 13 : 10" :h="inOrder ? 13 : 10"
        :src="avatarSrc" b="rounded-2" />
      <div flex flex-col items-start :text="inOrder ? 4 : 3"
        :style="{ color: inOrder ? lastCard?.color || 'gray' : 'gray', fontWeight: inOrder ? 'bold' : 'normal' }" m-l-2>
        <div display="none sm:block" max-w-12 max-w-20 overflow-hidden text-ellipsis>No.{{ id }}</div>
        <div min-w-8 max-w-20 overflow-hidden text-ellipsis>{{ name }}</div>
      </div>
    </div>
    <div flex items-center w-4 h-4 lh-4 justify="evenly" w="100%" m-t-1 m-b-1
      :style="{ color: lastCard?.color || 'gray' }">
      <i w-4 h-4 class="i mdi:cards-outline"></i>
      <div>{{ cardNum }}</div>
      <div w-6 h-6 border="rounded-2" :style="{ backgroundColor: lastCard?.color || 'gray' }">
        <i w-6 h-6 style="position:relative;left:4px" flex items-center justify="center" c='white'
          :class="iconClass"></i>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { generateFromString } from 'generate-avatar'
import { useRoomStore } from '~/store/room';
export interface PlayerProps {
  id: string,
  name: string,
  cardNum: number,
  lastCard: CardInfo | null
}

const props = withDefaults(defineProps<PlayerProps>(), {
  name: '无玩家',
  id: '-1',
  cardNum: 0,
  lastCard: null
})

const roomStore = useRoomStore()

const iconClass = computed(() => `i ${props.lastCard?.icon || 'pixelarticons:downasaur'}`)
const avatarSrc = computed(() => `data:image/svg+xml;utf8,${generateFromString(props.id + props.name)}`)
const inOrder = computed(() => {
  const idx = roomStore.players.findIndex(p => p.id === props.id && p.name === props.name)
  return idx === roomStore.order
})
</script>

<style scoped>
</style>
