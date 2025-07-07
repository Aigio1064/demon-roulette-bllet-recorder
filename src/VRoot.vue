<script setup lang="ts">
import { ref } from 'vue'

const bullet = {
    real: ref(0),
    empty: ref(0),
    converter: ref(false)
}
function shootReal() {
    if (bullet.converter.value) {
        if (bullet.empty.value <= 0) return
        bullet.empty.value -= 1
        bullet.converter.value = false
    } else {
        if (bullet.real.value <= 0) return
        bullet.real.value -= 1
    }
}
function shootEmpty() {
    if (bullet.converter.value) {
        if (bullet.real.value <= 0) return
        bullet.real.value -= 1
        bullet.converter.value = false
    } else {
        if (bullet.empty.value <= 0) return
        bullet.empty.value -= 1
    }
}
function useConverter() {
    bullet.converter.value = !bullet.converter.value
}
function setReal(value: string | number | Event) {
    let result: number
    if (value instanceof Event) {
        let e = value as unknown as Event & {
            target: HTMLInputElement & {
                value: string
            }
        }
        result = Number(e.target.value || 0)
    } else {
        result = Number(value)
    }
    bullet.real.value = result
}
function setEmpty(value: string | number | Event) {
    let result: number
    if (value instanceof Event) {
        let e = value as unknown as Event & {
            target: HTMLInputElement & {
                value: string
            }
        }
        result = Number(e.target.value || 0)
    } else {
        result = Number(value)
    }
    bullet.empty.value = result
}
function clear() {
    bullet.real.value = 0
    bullet.empty.value = 0
    bullet.converter.value = false
}
function submit(e: Event) {
    const t = e.target as any
    const real = t[2] as unknown as HTMLInputElement
    const empty = t[4] as unknown as HTMLInputElement
    setReal(real.value)
    setEmpty(empty.value)

}
</script>
<template>
    <form @submit.prevent="submit">
        <div>
            <button type="submit">设置</button>
            <button @click="clear">清空</button>
        </div>
        <div>
            <div>
                <span>实弹 [{{ bullet.real }}]</span>
                <input name="real" type="number" :value="bullet.real.value" min="0" max="8" />
                <button @click="shootReal">射击/抛壳</button>
            </div>
            <div>
                <span>空包 [{{ bullet.empty }}]</span>
                <input name="empty" type="number" :value="bullet.empty.value" min="0" max="8" />
                <button @click="shootEmpty">射击/抛壳</button>
            </div>
        </div>
        <div>
            <button @click="useConverter">使用转换器 [{{ bullet.converter.value ? '已使用' : '未使用' }}]</button>
        </div>
        <div>
            <a href="https://github.com/Aigio1064/demon-roulette-bllet-recorder">Aigio1064/demon-roulette-bllet-recorder</a>
        </div>
    </form>
</template>