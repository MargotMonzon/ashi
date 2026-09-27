<script setup>
import {
    onBeforeUnmount,
    onMounted,
    ref,
} from 'vue'

import { relationship } from '../data/relationship'

const sectionRef = ref(null)
const visibleItems = ref([])

let observer = null

onMounted(() => {
    observer = new IntersectionObserver(
        (entries) => {
            entries.forEach((entry) => {
                if (!entry.isIntersecting) {
                    return
                }

                const index = Number(
                    entry.target.dataset.index,
                )

                if (!visibleItems.value.includes(index)) {
                    visibleItems.value.push(index)
                }
            })
        },
        {
            threshold: 0.25,
        },
    )

    const items =
        sectionRef.value?.querySelectorAll(
            '.timeline__item',
        )

    items?.forEach((item) => {
        observer.observe(item)
    })
})

onBeforeUnmount(() => {
    observer?.disconnect()
})
</script>

<template>
    <section ref="sectionRef" id="historia" class="timeline">
        <div class="timeline__header">
            <span class="timeline__eyebrow">
                nosotros ♡
            </span>

            <h2>
                Nuestra
                <span>historia</span>
            </h2>

            <p>
                Recuerdo cada momento (aunque no lo creas), cada momento
                se queda marcado en mis recuerdos, son recuerdos muy valiosos
                y especiales para mi.
            </p>
        </div>

        <div class="timeline__content">
            <div class="timeline__line"></div>

            <article v-for="(moment, index) in relationship.timeline" :key="index" class="timeline__item" :class="{
                'timeline__item--reverse':
                    index % 2 !== 0,

                'timeline__item--visible':
                    visibleItems.includes(index),
            }" :data-index="index">
                <div class="timeline__marker">
                    <span></span>
                </div>

                <div class="timeline__photo-wrapper">
                    <div class="timeline__photo">
                        <img :src="moment.image" :alt="moment.title" />

                        <span class="timeline__photo-heart">
                            ♡
                        </span>
                    </div>
                </div>

                <div class="timeline__text">
                    <span class="timeline__date">
                        {{ moment.date }}
                    </span>

                    <h3>
                        {{ moment.title }}
                    </h3>

                    <p>
                        {{ moment.description }}
                    </p>
                </div>
            </article>

            <div class="timeline__ending">
                <span class="timeline__ending-heart">
                    ♥
                </span>

                <h3>
                    Y esto recién
                    <span>está comenzando.</span>
                </h3>

                <p>
                    Me debes una vida entera a
                    tu lado
                </p>
            </div>
        </div>

        <span class="
                timeline__decoration
                timeline__decoration--one
            ">
            ♡
        </span>

        <span class="
                timeline__decoration
                timeline__decoration--two
            ">
            ✦
        </span>

        <span class="
                timeline__decoration
                timeline__decoration--three
            ">
            ♡
        </span>
    </section>
</template>

<style scoped>
.timeline {
    position: relative;

    padding:
        120px 24px 140px;

    overflow: hidden;

    background:
        linear-gradient(180deg,
            #fffaf8 0%,
            #fdf4f2 45%,
            #fffaf8 100%);
}

/* ============================= */
/* HEADER */
/* ============================= */

.timeline__header {
    position: relative;

    z-index: 2;

    max-width: 600px;

    margin: 0 auto 100px;

    text-align: center;
}

.timeline__eyebrow {
    display: inline-block;

    margin-bottom: 24px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.timeline__header h2 {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: clamp(54px,
            10vw,
            90px);

    font-weight: 500;

    line-height: 0.88;
    letter-spacing: -0.04em;

    color: var(--wine);
}

.timeline__header h2 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.timeline__header p {
    max-width: 480px;

    margin: 30px auto 0;

    font-size: 14px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

/* ============================= */
/* TIMELINE */
/* ============================= */

.timeline__content {
    position: relative;

    z-index: 2;

    width: 100%;
    max-width: 1000px;

    margin: 0 auto;
}

.timeline__line {
    position: absolute;

    top: 0;
    bottom: 160px;
    left: 50%;

    width: 1px;

    background:
        linear-gradient(180deg,
            transparent,
            rgba(169, 74, 90, 0.28) 8%,
            rgba(169, 74, 90, 0.28) 92%,
            transparent);

    transform:
        translateX(-50%);
}

/* ============================= */
/* ITEM */
/* ============================= */

.timeline__item {
    position: relative;

    display: grid;

    grid-template-columns:
        1fr 80px 1fr;

    align-items: center;

    min-height: 450px;

    opacity: 0;

    transform:
        translateY(60px);

    transition:
        opacity 1s ease,
        transform 1s ease;
}

.timeline__item--visible {
    opacity: 1;

    transform:
        translateY(0);
}

.timeline__marker {
    position: relative;

    z-index: 5;

    grid-column: 2;

    display: flex;
    align-items: center;
    justify-content: center;

    grid-row: 1;
}

.timeline__marker span {
    width: 14px;
    height: 14px;

    border: 4px solid #fffaf8;

    border-radius: 50%;

    background: var(--red);

    box-shadow:
        0 0 0 1px rgba(169, 74, 90, 0.3);
}

/* ============================= */
/* FOTO */
/* ============================= */

.timeline__photo-wrapper {
    grid-column: 1;
    grid-row: 1;

    display: flex;
    justify-content: flex-end;

    padding-right: 45px;
}

.timeline__photo {
    position: relative;

    width: 290px;

    padding:
        12px 12px 48px;

    background: #fff;

    box-shadow:
        0 25px 60px rgba(107, 52, 62, 0.13);

    transform:
        rotate(-4deg);

    transition:
        transform 0.35s ease,
        box-shadow 0.35s ease;
}

.timeline__photo:hover {
    transform:
        rotate(0deg) scale(1.025);

    box-shadow:
        0 32px 70px rgba(107, 52, 62, 0.18);
}

.timeline__photo img {
    width: 100%;
    height: 310px;

    object-fit: cover;
}

.timeline__photo-heart {
    position: absolute;

    right: 20px;
    bottom: 12px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 25px;

    color: var(--red);
}

/* ============================= */
/* TEXTO */
/* ============================= */

.timeline__text {
    grid-column: 3;
    grid-row: 1;

    padding-left: 45px;
}

.timeline__date {
    display: block;

    margin-bottom: 12px;

    font-size: 9px;
    font-weight: 600;

    letter-spacing: 0.24em;
    text-transform: uppercase;

    color: var(--red);
}

.timeline__text h3 {
    max-width: 360px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 40px;
    font-weight: 500;

    line-height: 1;

    color: var(--wine);
}

.timeline__text p {
    max-width: 360px;

    margin-top: 20px;

    font-size: 13px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

/* ============================= */
/* ITEM INVERSO */
/* ============================= */

.timeline__item--reverse .timeline__photo-wrapper {
    grid-column: 3;

    justify-content: flex-start;

    padding-right: 0;
    padding-left: 45px;
}

.timeline__item--reverse .timeline__photo {
    transform:
        rotate(4deg);
}

.timeline__item--reverse .timeline__photo:hover {
    transform:
        rotate(0deg) scale(1.025);
}

.timeline__item--reverse .timeline__text {
    grid-column: 1;

    padding-right: 45px;
    padding-left: 0;

    text-align: right;
}

.timeline__item--reverse .timeline__text h3,
.timeline__item--reverse .timeline__text p {
    margin-left: auto;
}

/* ============================= */
/* FINAL */
/* ============================= */

.timeline__ending {
    position: relative;

    z-index: 3;

    max-width: 560px;

    margin: 100px auto 0;

    padding:
        55px 30px;

    text-align: center;

    background:
        rgba(255, 255, 255, 0.6);

    border:
        1px solid rgba(169, 74, 90, 0.1);

    border-radius:
        50px 8px 50px 8px;

    box-shadow:
        0 30px 80px rgba(107, 52, 62, 0.07);

    backdrop-filter:
        blur(12px);
}

.timeline__ending-heart {
    display: block;

    margin-bottom: 18px;

    font-size: 30px;

    color: var(--red);

    animation:
        heartbeat 2s ease-in-out infinite;
}

.timeline__ending h3 {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: clamp(38px,
            7vw,
            55px);

    font-weight: 500;

    line-height: 0.95;

    color: var(--wine);
}

.timeline__ending h3 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.timeline__ending p {
    margin-top: 20px;

    font-size: 13px;

    line-height: 1.8;

    color: var(--text-soft);
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.timeline__decoration {
    position: absolute;

    user-select: none;
    pointer-events: none;

    color:
        rgba(169,
            74,
            90,
            0.08);

    animation:
        float 7s ease-in-out infinite;
}

.timeline__decoration--one {
    top: 12%;
    left: 3%;

    font-size: 110px;

    transform:
        rotate(-20deg);
}

.timeline__decoration--two {
    top: 45%;
    right: 6%;

    font-size: 40px;

    animation-delay: -3s;
}

.timeline__decoration--three {
    right: 1%;
    bottom: 10%;

    font-size: 130px;

    animation-delay: -5s;

    transform:
        rotate(15deg);
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

@keyframes float {

    0%,
    100% {
        transform:
            translateY(0);
    }

    50% {
        transform:
            translateY(-16px);
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
}

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 760px) {
    .timeline {
        padding:
            100px 20px 120px;
    }

    .timeline__header {
        margin-bottom: 70px;
    }

    .timeline__line {
        left: 21px;

        transform: none;
    }

    .timeline__item,
    .timeline__item--reverse {
        display: grid;

        grid-template-columns:
            42px 1fr;

        min-height: auto;

        margin-bottom: 90px;
    }

    .timeline__marker,
    .timeline__item--reverse .timeline__marker {
        grid-column: 1;
        grid-row: 1 / span 2;

        align-self: start;

        padding-top: 30px;
    }

    .timeline__photo-wrapper,
    .timeline__item--reverse .timeline__photo-wrapper {
        grid-column: 2;
        grid-row: 1;

        justify-content: flex-start;

        padding: 0;
    }

    .timeline__photo {
        width: min(100%,
                300px);

        transform:
            rotate(-3deg);
    }

    .timeline__item--reverse .timeline__photo {
        transform:
            rotate(3deg);
    }

    .timeline__photo img {
        height: 300px;
    }

    .timeline__text,
    .timeline__item--reverse .timeline__text {
        grid-column: 2;
        grid-row: 2;

        padding:
            35px 0 0;

        text-align: left;
    }

    .timeline__item--reverse .timeline__text h3,
    .timeline__item--reverse .timeline__text p {
        margin-left: 0;
    }

    .timeline__text h3 {
        font-size: 35px;
    }

    .timeline__ending {
        margin-top: 40px;

        padding:
            45px 25px;
    }

    .timeline__decoration--one {
        left: -60px;
    }

    .timeline__decoration--three {
        right: -70px;
    }
}
</style>