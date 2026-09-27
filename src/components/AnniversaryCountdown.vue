<script setup>
import {
    computed,
    onBeforeUnmount,
    onMounted,
    ref,
} from 'vue'

import { relationship } from '../data/relationship'

const props = defineProps({
    isUnlocked: {
        type: Boolean,
        default: false,
    },
})

const now = ref(new Date())

let interval = null

onMounted(() => {
    interval = setInterval(() => {
        now.value = new Date()
    }, 1000)
})

onBeforeUnmount(() => {
    if (interval) {
        clearInterval(interval)
    }
})

/*
|--------------------------------------------------------------------------
| FECHA OBJETIVO
|--------------------------------------------------------------------------
|
| Fecha exacta a la que estamos contando.
|
*/
const targetDate = computed(() => {
    return new Date(
        relationship.anniversary.year,
        relationship.anniversary.month - 1,
        relationship.anniversary.day,
        0,
        0,
        0,
    )
})

/*
|--------------------------------------------------------------------------
| DIFERENCIA
|--------------------------------------------------------------------------
*/

const difference = computed(() => {
    return Math.max(
        0,
        targetDate.value.getTime() - now.value.getTime(),
    )
})

/*
|--------------------------------------------------------------------------
| CONTADOR
|--------------------------------------------------------------------------
*/

const countdown = computed(() => {
    const second = 1000
    const minute = second * 60
    const hour = minute * 60
    const day = hour * 24

    return {
        days: Math.floor(
            difference.value / day,
        ),

        hours: Math.floor(
            (difference.value % day) / hour,
        ),

        minutes: Math.floor(
            (difference.value % hour) / minute,
        ),

        seconds: Math.floor(
            (difference.value % minute) / second,
        ),
    }
})

const pad = (value) => {
    return String(value).padStart(2, '0')
}

/*
|--------------------------------------------------------------------------
| MESES JUNTOS
|--------------------------------------------------------------------------
*/

const monthsTogether = computed(() => {
    const start = new Date(
        relationship.relationshipStart.year,
        relationship.relationshipStart.month - 1,
        relationship.relationshipStart.day,
    )

    const target = targetDate.value

    return (
        (target.getFullYear() - start.getFullYear()) * 12 +
        (target.getMonth() - start.getMonth())
    )
})
</script>

<template>
    <section id="contador" class="countdown">
        <!-- ============================= -->
        <!-- DECORACIONES -->
        <!-- ============================= -->

        <!-- GATITOS DECORATIVOS -->

        <img src="/images/decorations/cat-paper-01.png" alt="" class="
                countdown__decoration-image
                countdown__decoration-image--one
            " />

        <!-- DECORACIONES ANTIGUAS -->

        <div class="countdown__background">
            <span class="countdown__heart countdown__heart--one">
                ♡
            </span>

            <span class="countdown__star countdown__star--one">
                ✦
            </span>
        </div>

        <div class="countdown__background">
            <span class="
                    countdown__heart
                    countdown__heart--one
                ">
                ♡
            </span>

            <span class="
                    countdown__heart
                    countdown__heart--two
                ">
                ♡
            </span>

            <span class="
                    countdown__star
                    countdown__star--one
                ">
                ✦
            </span>

            <span class="
                    countdown__star
                    countdown__star--two
                ">
                ✧
            </span>
        </div>

        <div class="countdown__content">

            <!-- ============================= -->
            <!-- ANTES DEL DESBLOQUEO -->
            <!-- ============================= -->

            <template v-if="!props.isUnlocked">
                <span class="countdown__eyebrow">
                    {{ relationship.intro.eyebrow }}
                </span>

                <p class="countdown__small">
                    faltan
                </p>

                <!-- CONTADOR -->

                <div class="countdown__numbers">
                    <div class="countdown__item">
                        <strong>
                            {{ pad(countdown.days) }}
                        </strong>

                        <span>
                            días
                        </span>
                    </div>

                    <span class="countdown__separator">
                        :
                    </span>

                    <div class="countdown__item">
                        <strong>
                            {{ pad(countdown.hours) }}
                        </strong>

                        <span>
                            horas
                        </span>
                    </div>

                    <span class="countdown__separator">
                        :
                    </span>

                    <div class="countdown__item">
                        <strong>
                            {{ pad(countdown.minutes) }}
                        </strong>

                        <span>
                            min
                        </span>
                    </div>

                    <span class="countdown__separator">
                        :
                    </span>

                    <div class="countdown__item">
                        <strong>
                            {{ pad(countdown.seconds) }}
                        </strong>

                        <span>
                            seg
                        </span>
                    </div>
                </div>

                <!-- DIVISOR -->

                <div class="countdown__divider">
                    <span></span>

                    <span class="countdown__divider-heart">
                        ♡
                    </span>

                    <span></span>
                </div>

                <!-- TEXTO -->

                <h2 class="countdown__title">
                    Un poquito menos

                    <span>
                        para nuestro día.
                    </span>
                </h2>

                <p class="countdown__description">
                    {{ relationship.intro.message }}
                </p>

                <!-- MESESITO -->

                <div class="countdown__years">
                    <span>
                        ✦
                    </span>

                    Este será nuestro mesesito número

                    <strong>
                        #{{ monthsTogether }}
                    </strong>

                    <span>
                        ✦
                    </span>
                </div>

                <!-- ============================= -->
                <!-- BLOQUEADO -->
                <!-- ============================= -->

                <div class="countdown__locked">
                    <div class="countdown__lock">
                        <span class="countdown__lock-body">
                            ♡
                        </span>

                        <span class="countdown__lock-icon">
                            🔒
                        </span>
                    </div>

                    <span class="countdown__locked-eyebrow">
                        todavía no...
                    </span>

                    <p>
                        Hay algo más esperando por ti.
                        Pero tendrás que volver cuando
                        este contador llegue a cero ♡
                    </p>
                </div>
            </template>

            <!-- ============================= -->
            <!-- DESBLOQUEADO -->
            <!-- ============================= -->

            <template v-else>
                <div class="anniversary">
                    <span class="anniversary__heart">
                        ♥
                    </span>

                    <span class="countdown__eyebrow">
                        por fin llegó
                    </span>

                    <h2>
                        Nuestro

                        <span>
                            mesesito #{{ monthsTogether }}
                        </span>
                    </h2>

                    <p>
                        10 meses eligiendonos día tras dia,
                        dias malos, dias buenos, pero siempre siendo
                        dos, un equipo.
                    </p>

                    <span class="anniversary__final">
                        Te amo ♡
                    </span>

                    <div class="anniversary__continue">
                        <span>
                            ahora sí...
                        </span>

                        <span class="anniversary__arrow">
                            ↓
                        </span>
                    </div>
                </div>
            </template>
        </div>
    </section>
</template>

<style scoped>
.countdown__decoration-image {
    position: absolute;

    z-index: 1;

    width: 170px;
    height: auto;

    user-select: none;
    pointer-events: none;

    opacity: 0.9;

    filter:
        drop-shadow(0 15px 25px rgba(107, 52, 62, 0.12));

    animation:
        catFloat 7s ease-in-out infinite;
}

.countdown__decoration-image--one {
    top: 10%;
    right: 4%;

    width: 180px;

    transform: rotate(8deg);
}

.countdown__decoration-image--two {
    bottom: 8%;
    left: 4%;

    width: 145px;

    transform: rotate(-8deg);

    animation-delay: -3s;
}

@keyframes catFloat {

    0%,
    100% {
        transform:
            translateY(0) rotate(5deg);
    }

    50% {
        transform:
            translateY(-12px) rotate(2deg);
    }
}

.countdown {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    min-height: 100dvh;

    padding: 100px 24px 80px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 20%,
            rgba(255, 255, 255, 0.95),
            transparent 32%),
        linear-gradient(180deg,
            #fffaf8 0%,
            #fdf5f2 48%,
            #f9e7e9 100%);
}

.countdown__content {
    position: relative;

    z-index: 2;

    width: 100%;
    max-width: 850px;

    text-align: center;

    animation: appear 1.1s ease both;
}

/* ============================= */
/* EYEBROW */
/* ============================= */

.countdown__eyebrow {
    display: inline-block;

    margin-bottom: 26px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.countdown__small {
    margin-bottom: 12px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 22px;
    font-style: italic;

    color: var(--text-soft);
}

/* ============================= */
/* CONTADOR */
/* ============================= */

.countdown__numbers {
    display: flex;
    align-items: flex-start;
    justify-content: center;

    gap: 20px;

    margin: 0 auto;
}

.countdown__item {
    display: flex;
    flex-direction: column;
    align-items: center;

    min-width: 100px;
}

.countdown__item strong {
    font-family: 'Cormorant Garamond', serif;

    font-size: clamp(64px,
            10vw,
            108px);

    font-weight: 500;

    line-height: 0.9;

    letter-spacing: -0.06em;

    color: var(--wine);
}

.countdown__item span {
    margin-top: 14px;

    font-size: 9px;
    font-weight: 600;

    letter-spacing: 0.22em;
    text-transform: uppercase;

    color: var(--text-soft);
}

.countdown__separator {
    margin-top: 10px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 58px;
    font-weight: 300;

    color: rgba(169, 74, 90, 0.28);

    animation:
        separatorBlink 1s ease-in-out infinite;
}

/* ============================= */
/* DIVISOR */
/* ============================= */

.countdown__divider {
    display: flex;
    align-items: center;
    justify-content: center;

    gap: 14px;

    max-width: 300px;

    margin: 56px auto 40px;
}

.countdown__divider>span:not(.countdown__divider-heart) {
    width: 100%;
    height: 1px;

    background:
        linear-gradient(90deg,
            transparent,
            rgba(169, 74, 90, 0.25));
}

.countdown__divider>span:last-child {
    background:
        linear-gradient(90deg,
            rgba(169, 74, 90, 0.25),
            transparent);
}

.countdown__divider-heart {
    font-size: 20px;

    color: var(--red);
}

/* ============================= */
/* TEXTO */
/* ============================= */

.countdown__title {
    font-family: 'Cormorant Garamond', serif;

    font-size: clamp(42px,
            8vw,
            70px);

    font-weight: 500;

    line-height: 0.95;

    color: var(--wine);
}

.countdown__title span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.countdown__description {
    max-width: 490px;

    margin: 28px auto 0;

    font-size: 14px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

/* ============================= */
/* MESESITO */
/* ============================= */

.countdown__years {
    display: inline-flex;
    align-items: center;

    gap: 12px;

    margin-top: 30px;

    padding: 11px 18px;

    border:
        1px solid rgba(169, 74, 90, 0.12);

    border-radius: 999px;

    background:
        rgba(255, 255, 255, 0.45);

    font-family: 'Cormorant Garamond', serif;

    font-size: 16px;

    color: var(--text-soft);

    backdrop-filter: blur(10px);
}

.countdown__years strong {
    font-weight: 600;

    color: var(--red);
}

.countdown__years>span {
    font-size: 8px;

    color: var(--pink);
}

/* ============================= */
/* BLOQUEADO */
/* ============================= */

.countdown__locked {
    display: flex;
    flex-direction: column;
    align-items: center;

    max-width: 360px;

    margin: 65px auto 0;
}

.countdown__lock {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    width: 74px;
    height: 74px;

    margin-bottom: 22px;

    border:
        1px solid rgba(169, 74, 90, 0.12);

    border-radius: 50%;

    background:
        rgba(255, 255, 255, 0.4);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.07);

    backdrop-filter: blur(10px);

    animation:
        lockFloat 3s ease-in-out infinite;
}

.countdown__lock-body {
    position: absolute;

    font-family: 'Cormorant Garamond', serif;

    font-size: 62px;

    color: rgba(169, 74, 90, 0.08);
}

.countdown__lock-icon {
    position: relative;

    z-index: 2;

    font-size: 22px;
}

.countdown__locked-eyebrow {
    margin-bottom: 10px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 22px;
    font-style: italic;

    color: var(--wine);
}

.countdown__locked p {
    font-size: 12px;
    font-weight: 300;

    line-height: 1.8;

    color:
        rgba(107,
            52,
            62,
            0.55);
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.countdown__heart,
.countdown__star {
    position: absolute;

    user-select: none;
    pointer-events: none;

    color:
        rgba(169,
            74,
            90,
            0.14);

    animation:
        float 6s ease-in-out infinite;
}

.countdown__heart--one {
    top: 18%;
    left: 8%;

    font-size: 70px;

    transform: rotate(-15deg);
}

.countdown__heart--two {
    right: 7%;
    bottom: 20%;

    font-size: 90px;

    animation-delay: -3s;

    transform: rotate(18deg);
}

.countdown__star--one {
    top: 22%;
    right: 15%;

    font-size: 26px;

    animation-delay: -1s;
}

.countdown__star--two {
    bottom: 25%;
    left: 15%;

    font-size: 18px;

    animation-delay: -2s;
}

/* ============================= */
/* DESBLOQUEADO */
/* ============================= */

.anniversary {
    max-width: 620px;

    margin: 0 auto;
}

.anniversary__heart {
    display: block;

    margin-bottom: 22px;

    font-size: 46px;

    color: var(--red);

    animation:
        heartbeat 1.8s ease-in-out infinite;
}

.anniversary h2 {
    margin-bottom: 30px;

    font-family: 'Cormorant Garamond', serif;

    font-size: clamp(58px,
            12vw,
            100px);

    font-weight: 500;

    line-height: 0.85;

    color: var(--wine);
}

.anniversary h2 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.anniversary p {
    max-width: 500px;

    margin: auto;

    font-size: 14px;

    line-height: 1.9;

    color: var(--text-soft);
}

.anniversary__final {
    display: block;

    margin-top: 36px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 24px;
    font-style: italic;

    color: var(--red);
}

.anniversary__continue {
    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 8px;

    margin-top: 70px;
}

.anniversary__continue>span:first-child {
    font-size: 9px;
    font-weight: 600;

    letter-spacing: 0.22em;
    text-transform: uppercase;

    color:
        rgba(107,
            52,
            62,
            0.45);
}

.anniversary__arrow {
    font-size: 20px;

    color: var(--red);

    animation:
        arrowDown 1.8s ease-in-out infinite;
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

@keyframes appear {
    from {
        opacity: 0;

        transform:
            translateY(30px);
    }

    to {
        opacity: 1;

        transform:
            translateY(0);
    }
}

@keyframes separatorBlink {

    0%,
    100% {
        opacity: 0.25;
    }

    50% {
        opacity: 1;
    }
}

@keyframes float {

    0%,
    100% {
        transform:
            translateY(0);
    }

    50% {
        transform:
            translateY(-15px);
    }
}

@keyframes heartbeat {

    0%,
    100% {
        transform:
            scale(1);
    }

    15% {
        transform:
            scale(1.15);
    }

    30% {
        transform:
            scale(1);
    }

    45% {
        transform:
            scale(1.1);
    }
}

@keyframes lockFloat {

    0%,
    100% {
        transform:
            translateY(0) scale(1);
    }

    50% {
        transform:
            translateY(-8px) scale(1.03);
    }
}

@keyframes arrowDown {

    0%,
    100% {
        opacity: 0.35;

        transform:
            translateY(0);
    }

    50% {
        opacity: 1;

        transform:
            translateY(8px);
    }
}

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 700px) {
    .countdown {
        padding:
            80px 18px 60px;
    }

    .countdown__numbers {
        gap: 7px;
    }

    .countdown__item {
        min-width: 58px;
    }

    .countdown__item strong {
        font-size:
            clamp(45px,
                15vw,
                70px);
    }

    .countdown__separator {
        margin-top: 4px;

        font-size: 40px;
    }

    .countdown__item span {
        margin-top: 10px;

        font-size: 7px;
    }

    .countdown__description {
        max-width: 330px;

        font-size: 13px;
    }

    .countdown__years {
        font-size: 14px;

        padding:
            10px 14px;
    }

    .countdown__locked {
        margin-top: 55px;
    }

    .countdown__heart--one {
        left: -15px;
    }

    .countdown__heart--two {
        right: -25px;
    }
}
</style>