<template>
    <div class="search-bar">
        <div :class="props.title || $slots.title ? 'title' : ''">
            <span v-if="props.title && !$slots.title">{{ props.title }}</span>
            <template v-else>
                <slot name="title"></slot>
            </template>
            <div v-if="props.title || $slots.title" @click="arrowClick" class="arrow-down">
                <el-icon>
                    <ArrowDown size="16" />
                </el-icon>
            </div>
        </div>
        <!-- <collapse-transition> -->
        <div ref="defaultSlot" class="default-slot-box default-slot-box-show" v-show="showBox">
            <slot></slot>
        </div>
        <!-- </collapse-transition> -->
    </div>
</template>

<script setup lang="ts">
    import { ref, onMounted } from 'vue'

    const props = defineProps({
        title: {
            type: String,
            default: ''
        },
        minHeight: {
            type: String,
            default: '40px'
        }
    })
    const defaultSlot = ref()
    let height = ref(0)
    const showBox = ref(true)
    let el: HTMLElement
    onMounted(() => {
        el = defaultSlot.value as HTMLElement
        height.value = el.clientHeight
        el.style.height = height.value + 'px'
    })
    // todo resize

    const arrowClick = () => {
        if (el.clientHeight > 0) {
            el.style.height = '0'
        } else {
            el.style.height = height.value + 'px'
        }
    }
</script>

<style lang="less" scoped>
    .search-bar {
        background-color: var(--section-bg-color);
        .default-slot-box {
            transition: all 0.3s;
        }
    }
    .title {
        position: relative;
        font-size: 16px;
        min-height: v-bind('props.minHeight');
        border-bottom: 1px solid var(--default-border-color);
        padding: 0 10px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .arrow-down {
            position: absolute;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            padding: 8px;
            cursor: pointer;
        }
    }
    :slotted(.search-box) {
        display: flex;
        flex-wrap: wrap;
        padding: 10px;
        & .el-input {
            margin: 0 20px 10px 0;
        }
    }
</style>
