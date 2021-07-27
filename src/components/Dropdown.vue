<template>
  <div class="dropdown" ref="dropdownRef">
    <a
      href="#"
      class="btn btn-outline-light my-2 dropdown-toggle"
      @click.prevent="toggleOpen"
    >
      {{ title }}
    </a>
    <ul class="dropdown-menu" :style="{ display: 'block' }" v-if="isOpen">
      <slot></slot>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted, ref, watch } from 'vue'
import useClickOutside from '../hooks/useClickOutside'

export default defineComponent({
  name: 'Dropdown',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  setup() {
    const isOpen = ref(false)
    const toggleOpen = () => {
      isOpen.value = !isOpen.value
    }

    const dropdownRef = ref<null | HTMLElement>(null)
    const isClickOutside = useClickOutside(dropdownRef)
    watch(isClickOutside,() => {
      if (isOpen.value && isClickOutside.value){
        // console.log(isClickOutside)
        isOpen.value = false
      }
    })

    //点击外部隐藏
    //获取dom节点并return
    // const dropdownRef = ref<null | HTMLElement>(null)
    // const handler = (e: MouseEvent) => {
    //   if(dropdownRef.value) {
    //     console.log(dropdownRef.value)
    //     if(!dropdownRef.value.contains(e.target as HTMLElement) && isOpen.value){
    //       isOpen.value = false
    //     }
    //   }
    // }
    // onMounted(()=>{
    //   document.addEventListener('click',handler)
    // })
    // onUnmounted(()=>{
    //   document.removeEventListener('click',handler)
    // })
    return {
      isOpen,
      toggleOpen,
      dropdownRef
    }
  },
})
</script>
