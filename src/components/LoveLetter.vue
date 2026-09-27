<script setup>
import { ref } from 'vue'

import { relationship } from '../data/relationship'

const isOpened = ref(false)

const openLetter = () => {
    if (isOpened.value) {
        return
    }

    isOpened.value = true
}
</script>

<template>
    <section id="carta" class="love-letter" :class="{
        'love-letter--opened': isOpened,
    }">
        <!-- DECORACIONES -->

        <span class="
                love-letter__decoration
                love-letter__decoration--one
            ">
            ♡
        </span>

        <span class="
                love-letter__decoration
                love-letter__decoration--two
            ">
            ✦
        </span>

        <span class="
                love-letter__decoration
                love-letter__decoration--three
            ">
            ♡
        </span>

        <div class="love-letter__header">
            <span class="love-letter__eyebrow">
                {{ relationship.letter.eyebrow }}
            </span>

            <h2>
                Hay algo que
                <span>quiero decirte.</span>
            </h2>

            <p v-if="!isOpened">
                Esta sí tienes que abrirla tú.
            </p>
        </div>

        <!-- ============================= -->
        <!-- SOBRE -->
        <!-- ============================= -->

        <div class="envelope-area">
            <button type="button" class="envelope" :class="{
                'envelope--opened': isOpened,
            }" :aria-label="isOpened
                ? 'Carta abierta'
                : 'Abrir carta'
                " @click="openLetter">
                <!-- CARTA QUE SALE DEL SOBRE -->

                <div class="envelope__paper">
                    <span>
                        {{ relationship.letter.title }}
                    </span>

                    <small>
                        ♡
                    </small>
                </div>

                <!-- PARTE TRASERA -->

                <div class="envelope__back"></div>

                <!-- CARTA FRONTAL -->

                <div class="envelope__front">
                    <div class="envelope__front-left"></div>

                    <div class="envelope__front-right"></div>

                    <div class="envelope__front-bottom"></div>
                </div>

                <!-- SOLAPA -->

                <div class="envelope__flap"></div>

                <!-- SELLO -->

                <div class="envelope__seal">
                    ♥
                </div>
            </button>

            <div v-if="!isOpened" class="envelope-area__hint">
                <span>
                    toca el sobre
                </span>

                <span>
                    ↑
                </span>
            </div>
        </div>

        <!-- ============================= -->
        <!-- CARTA COMPLETA -->
        <!-- ============================= -->

        <Transition name="letter">
            <article v-if="isOpened" class="letter-paper">
                <span class="letter-paper__quote">
                    “
                </span>

                <div class="letter-paper__top">
                    <span>
                        para ti
                    </span>

                    <span>
                        ♡
                    </span>
                </div>

                <h3>
                    {{ relationship.letter.greeting }}
                </h3>

                <div class="letter-paper__content">
                    <p v-for="(
paragraph,
    index
                        ) in relationship.letter.paragraphs" :key="index">
                        {{ paragraph }}
                    </p>
                </div>

                <div class="letter-paper__closing">
                    <p>
                        {{ relationship.letter.closing }}
                    </p>

                    <span>
                        — {{ relationship.letter.signature }} ♡
                    </span>
                </div>

                <div class="letter-paper__little-heart">
                    ♥
                </div>
            </article>
        </Transition>

        <!-- CONTINUAR -->

        <Transition name="continue">
            <div v-if="isOpened" class="love-letter__continue">
                <span>
                    todavía queda una última cosita...
                </span>

                <span class="love-letter__arrow">
                    ↓
                </span>
            </div>
        </Transition>
    </section>
</template>

<style scoped>
.love-letter {
    position: relative;

    min-height: 100dvh;

    padding:
        130px 24px 150px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 20%,
            rgba(255, 255, 255, 0.96),
            transparent 32%),
        linear-gradient(180deg,
            #fffaf8 0%,
            #f8e4e6 50%,
            #fffaf8 100%);
}

/* ============================= */
/* HEADER */
/* ============================= */

.love-letter__header {
    position: relative;

    z-index: 5;

    max-width: 650px;

    margin: 0 auto 70px;

    text-align: center;
}

.love-letter__eyebrow {
    display: inline-block;

    margin-bottom: 24px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.love-letter__header h2 {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size:
        clamp(52px,
            10vw,
            88px);

    font-weight: 500;

    line-height: 0.9;
    letter-spacing: -0.04em;

    color: var(--wine);
}

.love-letter__header h2 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.love-letter__header p {
    margin-top: 25px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 18px;
    font-style: italic;

    color:
        rgba(107,
            52,
            62,
            0.55);
}

/* ============================= */
/* ÁREA SOBRE */
/* ============================= */

.envelope-area {
    position: relative;

    z-index: 6;

    display: flex;
    flex-direction: column;
    align-items: center;

    min-height: 330px;
}

.envelope {
    position: relative;

    width: 360px;
    height: 230px;

    margin-top: 40px;

    padding: 0;

    border: 0;

    background: transparent;

    cursor: pointer;

    perspective: 1000px;

    filter:
        drop-shadow(0 30px 30px rgba(107, 52, 62, 0.16));

    transition:
        transform 0.35s ease;
}

.envelope:not(.envelope--opened):hover {
    transform:
        translateY(-7px) rotate(-1deg);
}

/* ============================= */
/* SOBRE BACK */
/* ============================= */

.envelope__back {
    position: absolute;

    inset: 0;

    border-radius:
        5px 5px 14px 14px;

    background:
        #eabfc4;
}

/* ============================= */
/* PAPEL INTERIOR */
/* ============================= */

.envelope__paper {
    position: absolute;

    z-index: 2;

    top: 15px;
    left: 50%;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    width: 88%;
    height: 190px;

    border:
        1px solid rgba(169,
            74,
            90,
            0.08);

    border-radius: 4px;

    background:
        linear-gradient(180deg,
            #fffdfc,
            #fff9f7);

    transform:
        translateX(-50%);

    transition:
        transform 1s cubic-bezier(0.2,
            0.8,
            0.2,
            1);

    box-shadow:
        0 10px 30px rgba(107,
            52,
            62,
            0.08);
}

.envelope__paper span {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 31px;
    font-style: italic;

    color: var(--wine);
}

.envelope__paper small {
    margin-top: 8px;

    font-size: 20px;

    color: var(--red);
}

/* ============================= */
/* FRENTE DEL SOBRE */
/* ============================= */

.envelope__front {
    position: absolute;

    z-index: 4;

    inset: 0;

    overflow: hidden;

    border-radius:
        5px 5px 14px 14px;
}

.envelope__front-left {
    position: absolute;

    bottom: 0;
    left: 0;

    width: 0;
    height: 0;

    border-top:
        115px solid transparent;

    border-bottom:
        115px solid #efcbd0;

    border-right:
        180px solid transparent;
}

.envelope__front-right {
    position: absolute;

    right: 0;
    bottom: 0;

    width: 0;
    height: 0;

    border-top:
        115px solid transparent;

    border-bottom:
        115px solid #e6b8be;

    border-left:
        180px solid transparent;
}

.envelope__front-bottom {
    position: absolute;

    bottom: 0;
    left: 50%;

    width: 0;
    height: 0;

    border-right:
        180px solid transparent;

    border-bottom:
        115px solid #f4d6d9;

    border-left:
        180px solid transparent;

    transform:
        translateX(-50%);
}

/* ============================= */
/* SOLAPA */
/* ============================= */

.envelope__flap {
    position: absolute;

    z-index: 6;

    top: 0;
    left: 0;

    width: 0;
    height: 0;

    border-top:
        120px solid #e5b3ba;

    border-right:
        180px solid transparent;

    border-left:
        180px solid transparent;

    transform-origin:
        top center;

    transform:
        rotateX(0deg);

    transition:
        transform 0.8s cubic-bezier(0.2,
            0.8,
            0.2,
            1);

    backface-visibility: hidden;
}

/* ============================= */
/* SELLO */
/* ============================= */

.envelope__seal {
    position: absolute;

    z-index: 8;

    top: 90px;
    left: 50%;

    display: flex;
    align-items: center;
    justify-content: center;

    width: 46px;
    height: 46px;

    border-radius: 50%;

    background:
        var(--red);

    color: white;

    font-size: 18px;

    transform:
        translateX(-50%);

    box-shadow:
        0 8px 20px rgba(107,
            52,
            62,
            0.22);

    transition:
        opacity 0.3s ease,
        transform 0.3s ease;
}

/* ============================= */
/* SOBRE ABIERTO */
/* ============================= */

.envelope--opened {
    cursor: default;
}

.envelope--opened .envelope__flap {
    z-index: 1;

    transform:
        rotateX(180deg);
}

.envelope--opened .envelope__seal {
    opacity: 0;

    transform:
        translateX(-50%) scale(0.5);
}

.envelope--opened .envelope__paper {
    transform:
        translateX(-50%) translateY(-125px);
}

/* ============================= */
/* HINT */
/* ============================= */

.envelope-area__hint {
    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 5px;

    margin-top: 20px;

    color:
        rgba(107,
            52,
            62,
            0.45);
}

.envelope-area__hint span:first-child {
    font-size: 8px;
    font-weight: 600;

    letter-spacing: 0.22em;
    text-transform: uppercase;
}

.envelope-area__hint span:last-child {
    font-size: 18px;

    color: var(--red);

    animation:
        hintUp 1.8s ease-in-out infinite;
}

/* ============================= */
/* CARTA */
/* ============================= */

.letter-paper {
    position: relative;

    z-index: 7;

    width: 100%;
    max-width: 680px;

    margin:
        40px auto 0;

    padding:
        65px 70px;

    border:
        1px solid rgba(169,
            74,
            90,
            0.1);

    border-radius:
        6px 48px 6px 48px;

    background:
        rgba(255,
            253,
            252,
            0.88);

    box-shadow:
        0 35px 90px rgba(107,
            52,
            62,
            0.1);

    backdrop-filter:
        blur(15px);
}

.letter-paper::before {
    content: '';

    position: absolute;

    inset: 10px;

    border:
        1px solid rgba(169,
            74,
            90,
            0.06);

    border-radius:
        4px 40px 4px 40px;

    pointer-events: none;
}

.letter-paper__quote {
    position: absolute;

    top: 8px;
    left: 35px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 110px;

    line-height: 1;

    color:
        rgba(169,
            74,
            90,
            0.07);
}

.letter-paper__top {
    position: relative;

    z-index: 2;

    display: flex;
    align-items: center;
    justify-content: space-between;

    margin-bottom: 45px;

    padding-bottom: 15px;

    border-bottom:
        1px solid rgba(169,
            74,
            90,
            0.1);
}

.letter-paper__top span:first-child {
    font-size: 8px;
    font-weight: 600;

    letter-spacing: 0.25em;
    text-transform: uppercase;

    color: var(--red);
}

.letter-paper__top span:last-child {
    color: var(--red);
}

.letter-paper h3 {
    position: relative;

    z-index: 2;

    margin-bottom: 35px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size:
        clamp(36px,
            6vw,
            48px);

    font-style: italic;
    font-weight: 500;

    color: var(--wine);
}

.letter-paper__content {
    position: relative;

    z-index: 2;
}

.letter-paper__content p {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 19px;
    font-weight: 400;

    line-height: 1.9;

    color: var(--text-soft);
}

.letter-paper__content p+p {
    margin-top: 24px;
}

.letter-paper__closing {
    position: relative;

    z-index: 2;

    margin-top: 50px;

    text-align: right;
}

.letter-paper__closing p {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 20px;
    font-style: italic;

    color: var(--wine);
}

.letter-paper__closing span {
    display: block;

    margin-top: 12px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 24px;
    font-style: italic;

    color: var(--red);
}

.letter-paper__little-heart {
    margin-top: 45px;

    text-align: center;

    font-size: 18px;

    color: var(--red);

    animation:
        heartbeat 2s ease-in-out infinite;
}

/* ============================= */
/* CONTINUAR */
/* ============================= */

.love-letter__continue {
    position: relative;

    z-index: 6;

    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 12px;

    margin-top: 80px;

    text-align: center;
}

.love-letter__continue span:first-child {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 18px;
    font-style: italic;

    color:
        rgba(107,
            52,
            62,
            0.58);
}

.love-letter__arrow {
    font-size: 20px;

    color: var(--red);

    animation:
        arrowDown 1.8s ease-in-out infinite;
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.love-letter__decoration {
    position: absolute;

    user-select: none;
    pointer-events: none;

    color:
        rgba(169,
            74,
            90,
            0.08);

    animation:
        floating 7s ease-in-out infinite;
}

.love-letter__decoration--one {
    top: 13%;
    left: 5%;

    font-size: 100px;
}

.love-letter__decoration--two {
    top: 40%;
    right: 8%;

    font-size: 32px;

    animation-delay: -2s;
}

.love-letter__decoration--three {
    right: 2%;
    bottom: 8%;

    font-size: 130px;

    animation-delay: -4s;
}

/* ============================= */
/* TRANSICIONES */
/* ============================= */

.letter-enter-active {
    transition:
        opacity 1s ease 0.3s,
        transform 1s ease 0.3s;
}

.letter-enter-from {
    opacity: 0;

    transform:
        translateY(60px);
}

.letter-enter-to {
    opacity: 1;

    transform:
        translateY(0);
}

.continue-enter-active {
    transition:
        opacity 0.8s ease 0.9s;
}

.continue-enter-from {
    opacity: 0;
}

.continue-enter-to {
    opacity: 1;
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

@keyframes hintUp {

    0%,
    100% {
        transform:
            translateY(5px);

        opacity: 0.4;
    }

    50% {
        transform:
            translateY(-4px);

        opacity: 1;
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
            scale(1.18);
    }

    30% {
        transform:
            scale(1);
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

@keyframes floating {

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

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 600px) {
    .love-letter {
        padding:
            100px 20px 120px;
    }

    .love-letter__header {
        margin-bottom: 50px;
    }

    .envelope-area {
        min-height: 280px;
    }

    .envelope {
        width: 300px;
        height: 190px;

        margin-top: 35px;
    }

    .envelope__paper {
        height: 155px;
    }

    .envelope__paper span {
        font-size: 27px;
    }

    .envelope__front-left {
        border-top-width: 95px;
        border-bottom-width: 95px;
        border-right-width: 150px;
    }

    .envelope__front-right {
        border-top-width: 95px;
        border-bottom-width: 95px;
        border-left-width: 150px;
    }

    .envelope__front-bottom {
        border-right-width: 150px;
        border-bottom-width: 95px;
        border-left-width: 150px;
    }

    .envelope__flap {
        border-top-width: 100px;
        border-right-width: 150px;
        border-left-width: 150px;
    }

    .envelope__seal {
        top: 72px;

        width: 42px;
        height: 42px;
    }

    .envelope--opened .envelope__paper {
        transform:
            translateX(-50%) translateY(-105px);
    }

    .letter-paper {
        margin-top: 20px;

        padding:
            50px 26px;

        border-radius:
            5px 32px 5px 32px;
    }

    .letter-paper::before {
        border-radius:
            3px 25px 3px 25px;
    }

    .letter-paper__content p {
        font-size: 17px;

        line-height: 1.8;
    }

    .letter-paper__closing {
        margin-top: 40px;
    }

    .love-letter__decoration--one {
        left: -50px;
    }

    .love-letter__decoration--three {
        right: -70px;
    }
}
</style>