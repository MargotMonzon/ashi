<script setup>
import { ref } from 'vue'
import { relationship } from '../data/relationship'

const openedCards = ref([])

const toggleCard = (index) => {
    if (openedCards.value.includes(index)) {
        openedCards.value = openedCards.value.filter(
            item => item !== index,
        )

        return
    }

    openedCards.value.push(index)
}

const isOpen = (index) => {
    return openedCards.value.includes(index)
}
</script>

<template>
    <section id="razones" class="reasons">
        <div class="reasons__header">
            <span class="reasons__eyebrow">
                algunas cositas ♡
            </span>

            <h2>
                Cosas que
                <span>amo de ti</span>
            </h2>

            <p>
                En realidad amo todo de ti, si me pongo a enumerar
                cada cosa, nunca tendría fin, estoy completamente enamorada
                de tiii.
            </p>

            <span class="reasons__hint">
                toca cada tarjeta para descubrirla
            </span>
        </div>

        <div class="reasons__grid">
            <button v-for="(reason, index) in relationship.reasons" :key="index" type="button" class="reason-card"
                :class="{
                    'reason-card--open': isOpen(index),
                }" @click="toggleCard(index)">
                <div class="reason-card__inner">

                    <!-- FRENTE -->
                    <div class="reason-card__face reason-card__front">
                        <span class="reason-card__number">
                            {{ reason.number }}
                        </span>

                        <span class="reason-card__heart">
                            ♡
                        </span>

                        <span class="reason-card__touch">
                            tocar para abrir
                        </span>
                    </div>

                    <!-- REVERSO -->
                    <div class="reason-card__face reason-card__back">
                        <span class="reason-card__small-heart">
                            ♥
                        </span>

                        <h3>
                            {{ reason.title }}
                        </h3>

                        <p>
                            {{ reason.text }}
                        </p>

                        <span class="reason-card__close">
                            tocar para cerrar
                        </span>
                    </div>

                </div>
            </button>
        </div>

        <div class="reasons__ending">
            <span>
                y podría seguir...
            </span>

            <strong>
                pero probablemente nunca terminaría ♡
            </strong>
        </div>

        <span class="reasons__decoration reasons__decoration--one">
            ♡
        </span>

        <span class="reasons__decoration reasons__decoration--two">
            ✦
        </span>

        <span class="reasons__decoration reasons__decoration--three">
            ♡
        </span>
    </section>
</template>

<style scoped>
.reasons {
    position: relative;

    min-height: 100vh;

    padding:
        130px 24px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 20%,
            rgba(255, 255, 255, 0.9),
            transparent 32%),
        linear-gradient(180deg,
            #fffaf8 0%,
            #f9e7e9 50%,
            #fffaf8 100%);
}

/* ============================= */
/* HEADER */
/* ============================= */

.reasons__header {
    position: relative;

    z-index: 2;

    max-width: 620px;

    margin: 0 auto 70px;

    text-align: center;
}

.reasons__eyebrow {
    display: inline-block;

    margin-bottom: 24px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.reasons__header h2 {
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

.reasons__header h2 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.reasons__header p {
    max-width: 450px;

    margin: 28px auto 0;

    font-size: 14px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

.reasons__hint {
    display: inline-block;

    margin-top: 24px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 16px;
    font-style: italic;

    color:
        rgba(107,
            52,
            62,
            0.5);
}

/* ============================= */
/* GRID */
/* ============================= */

.reasons__grid {
    position: relative;

    z-index: 2;

    display: grid;

    grid-template-columns:
        repeat(3,
            minmax(0, 1fr));

    gap: 20px;

    width: 100%;
    max-width: 950px;

    margin: 0 auto;
}

/* ============================= */
/* CARD */
/* ============================= */

.reason-card {
    display: block;

    width: 100%;
    height: 320px;

    padding: 0;

    border: none;

    background: transparent;

    perspective: 1200px;

    cursor: pointer;
}

.reason-card__inner {
    position: relative;

    width: 100%;
    height: 100%;

    transform-style: preserve-3d;

    transition:
        transform 0.8s cubic-bezier(0.2,
            0.7,
            0.2,
            1);
}

.reason-card--open .reason-card__inner {
    transform:
        rotateY(180deg);
}

.reason-card__face {
    position: absolute;

    inset: 0;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    padding: 32px;

    border:
        1px solid rgba(169,
            74,
            90,
            0.1);

    border-radius:
        8px 38px 8px 38px;

    backface-visibility: hidden;

    overflow: hidden;
}

/* ============================= */
/* FRENTE */
/* ============================= */

.reason-card__front {
    background:
        rgba(255,
            255,
            255,
            0.62);

    box-shadow:
        0 25px 60px rgba(107,
            52,
            62,
            0.08);

    backdrop-filter:
        blur(12px);
}

.reason-card__front::before {
    content: '';

    position: absolute;

    width: 160px;
    height: 160px;

    border-radius: 50%;

    background:
        radial-gradient(circle,
            rgba(244,
                217,
                220,
                0.5),
            transparent 70%);
}

.reason-card__number {
    position: relative;

    z-index: 2;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 76px;
    font-weight: 400;

    line-height: 1;

    color: var(--wine);
}

.reason-card__heart {
    position: relative;

    z-index: 2;

    margin-top: 10px;

    font-size: 26px;

    color: var(--red);

    transition:
        transform 0.3s ease;
}

.reason-card:hover .reason-card__heart {
    transform:
        scale(1.2);
}

.reason-card__touch {
    position: absolute;

    bottom: 24px;

    font-size: 8px;
    font-weight: 600;

    letter-spacing: 0.2em;
    text-transform: uppercase;

    color:
        rgba(107,
            52,
            62,
            0.38);
}

/* ============================= */
/* ATRÁS */
/* ============================= */

.reason-card__back {
    background:
        linear-gradient(145deg,
            #fff 0%,
            #fcebed 100%);

    box-shadow:
        0 25px 60px rgba(107,
            52,
            62,
            0.12);

    transform:
        rotateY(180deg);
}

.reason-card__small-heart {
    margin-bottom: 18px;

    font-size: 22px;

    color: var(--red);

    animation:
        heartbeat 2s ease-in-out infinite;
}

.reason-card__back h3 {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 32px;
    font-weight: 500;

    line-height: 1;

    color: var(--wine);
}

.reason-card__back p {
    margin-top: 18px;

    font-size: 12px;
    font-weight: 300;

    line-height: 1.8;

    color: var(--text-soft);
}

.reason-card__close {
    position: absolute;

    bottom: 22px;

    font-size: 7px;
    font-weight: 600;

    letter-spacing: 0.18em;
    text-transform: uppercase;

    color:
        rgba(107,
            52,
            62,
            0.3);
}

/* ============================= */
/* FINAL */
/* ============================= */

.reasons__ending {
    position: relative;

    z-index: 2;

    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 4px;

    margin-top: 70px;

    text-align: center;
}

.reasons__ending span {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 20px;

    color: var(--text-soft);
}

.reasons__ending strong {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 26px;
    font-style: italic;
    font-weight: 500;

    color: var(--red);
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.reasons__decoration {
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

.reasons__decoration--one {
    top: 15%;
    left: 4%;

    font-size: 100px;
}

.reasons__decoration--two {
    top: 48%;
    right: 5%;

    font-size: 34px;

    animation-delay: -2s;
}

.reasons__decoration--three {
    right: 1%;
    bottom: 10%;

    font-size: 120px;

    animation-delay: -4s;
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

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

@keyframes float {

    0%,
    100% {
        transform:
            translateY(0);
    }

    50% {
        transform:
            translateY(-14px);
    }
}

/* ============================= */
/* TABLET */
/* ============================= */

@media (max-width: 900px) {
    .reasons__grid {
        grid-template-columns:
            repeat(2,
                minmax(0, 1fr));
    }
}

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 600px) {
    .reasons {
        padding:
            100px 20px;
    }

    .reasons__header {
        margin-bottom: 55px;
    }

    .reasons__grid {
        grid-template-columns:
            repeat(2,
                minmax(0, 1fr));

        gap: 12px;
    }

    .reason-card {
        height: 235px;
    }

    .reason-card__face {
        padding:
            22px 16px;

        border-radius:
            6px 28px 6px 28px;
    }

    .reason-card__number {
        font-size: 56px;
    }

    .reason-card__heart {
        font-size: 22px;
    }

    .reason-card__back h3 {
        font-size: 25px;
    }

    .reason-card__back p {
        margin-top: 12px;

        font-size: 11px;

        line-height: 1.65;
    }

    .reason-card__touch,
    .reason-card__close {
        font-size: 6px;
    }

    .reasons__decoration--one {
        left: -50px;
    }

    .reasons__decoration--three {
        right: -60px;
    }
}
</style>