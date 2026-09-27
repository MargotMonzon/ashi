<script setup>
import { ref } from 'vue'

const emit = defineEmits(['open'])

const isOpening = ref(false)

const openGift = () => {
    if (isOpening.value) return

    isOpening.value = true

    setTimeout(() => {
        emit('open')
    }, 900)
}
</script>

<template>
    <section class="intro" :class="{ 'intro--opening': isOpening }">
        <div class="intro__decoration intro__decoration--one">
            ♡
        </div>

        <div class="intro__decoration intro__decoration--two">
            ♡
        </div>

        <div class="intro__decoration intro__decoration--three">
            ✦
        </div>

        <div class="intro__decoration intro__decoration--four">
            ✧
        </div>

        <div class="intro__content">
            <span class="intro__eyebrow">
                una pequeña sorpresa
            </span>

            <div class="intro__heart">
                <span>♥</span>
            </div>

            <h1 class="intro__title">
                Tengo algo
                <span>para ti</span>
            </h1>

            <p class="intro__description">
                Hice este pequeño lugar pensando en ti,
                en nosotras y en todas las cosas bonitas
                que todavía nos quedan por vivir.
            </p>

            <button class="intro__button" type="button" @click="openGift">
                <span>Abrir mi regalo</span>

                <span class="intro__button-heart">
                    ♡
                </span>
            </button>

            <p class="intro__hint">
                hecho con mucho amor para ti
            </p>
        </div>
    </section>
</template>

<style scoped>
.intro {
    position: fixed;
    inset: 0;

    z-index: 1000;

    display: flex;
    align-items: center;
    justify-content: center;

    min-height: 100dvh;

    padding: 32px 24px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 20%,
            rgba(255, 255, 255, 0.95),
            transparent 36%),
        linear-gradient(160deg,
            #fffaf8 0%,
            #fcecee 52%,
            #f5dcdf 100%);

    transition:
        opacity 0.9s ease,
        transform 0.9s ease,
        filter 0.9s ease;
}

.intro--opening {
    opacity: 0;

    transform: scale(1.08);

    filter: blur(12px);

    pointer-events: none;
}

.intro__content {
    position: relative;

    z-index: 2;

    width: 100%;
    max-width: 520px;

    text-align: center;

    animation: introAppear 1.2s ease both;
}

.intro__eyebrow {
    display: inline-block;

    margin-bottom: 28px;

    font-size: 11px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.intro__heart {
    display: flex;
    align-items: center;
    justify-content: center;

    width: 76px;
    height: 76px;

    margin: 0 auto 26px;

    border: 1px solid rgba(169, 74, 90, 0.18);

    border-radius: 50%;

    background: rgba(255, 255, 255, 0.48);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.08),
        inset 0 0 30px rgba(255, 255, 255, 0.8);

    backdrop-filter: blur(10px);
}

.intro__heart span {
    display: block;

    font-size: 28px;

    color: var(--red);

    animation: heartbeat 2s ease-in-out infinite;
}

.intro__title {
    margin-bottom: 20px;

    font-family: 'Cormorant Garamond', serif;

    font-size: clamp(52px, 14vw, 82px);
    font-weight: 500;

    line-height: 0.88;
    letter-spacing: -0.04em;

    color: var(--wine);
}

.intro__title span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.intro__description {
    max-width: 390px;

    margin: 28px auto 34px;

    font-size: 14px;
    font-weight: 300;

    line-height: 1.8;

    color: var(--text-soft);
}

.intro__button {
    position: relative;

    display: inline-flex;
    align-items: center;
    justify-content: center;

    gap: 14px;

    min-width: 220px;

    padding: 16px 28px;

    border: 1px solid rgba(169, 74, 90, 0.15);

    border-radius: 999px;

    background: var(--wine);

    color: white;

    cursor: pointer;

    box-shadow:
        0 14px 40px rgba(107, 52, 62, 0.22);

    transition:
        transform 0.25s ease,
        box-shadow 0.25s ease,
        background 0.25s ease;
}

.intro__button:hover {
    transform: translateY(-3px);

    background: var(--red);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.28);
}

.intro__button:active {
    transform: scale(0.97);
}

.intro__button span:first-child {
    font-size: 13px;
    font-weight: 500;

    letter-spacing: 0.04em;
}

.intro__button-heart {
    font-size: 18px;
}

.intro__hint {
    margin-top: 20px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 16px;
    font-style: italic;

    color: rgba(107, 52, 62, 0.55);
}

.intro__decoration {
    position: absolute;

    user-select: none;
    pointer-events: none;

    color: rgba(169, 74, 90, 0.2);

    animation: floating 6s ease-in-out infinite;
}

.intro__decoration--one {
    top: 15%;
    left: 12%;

    font-size: 46px;

    transform: rotate(-18deg);
}

.intro__decoration--two {
    right: 10%;
    bottom: 15%;

    font-size: 58px;

    animation-delay: -2s;

    transform: rotate(16deg);
}

.intro__decoration--three {
    top: 22%;
    right: 16%;

    font-size: 24px;

    animation-delay: -1s;
}

.intro__decoration--four {
    bottom: 24%;
    left: 16%;

    font-size: 20px;

    animation-delay: -3s;
}

@keyframes introAppear {
    from {
        opacity: 0;

        transform: translateY(30px);
    }

    to {
        opacity: 1;

        transform: translateY(0);
    }
}

@keyframes heartbeat {

    0%,
    100% {
        transform: scale(1);
    }

    12% {
        transform: scale(1.15);
    }

    22% {
        transform: scale(1);
    }

    34% {
        transform: scale(1.1);
    }

    44% {
        transform: scale(1);
    }
}

@keyframes floating {

    0%,
    100% {
        transform: translateY(0) rotate(-5deg);
    }

    50% {
        transform: translateY(-14px) rotate(5deg);
    }
}

@media (max-width: 600px) {
    .intro {
        padding-inline: 22px;
    }

    .intro__description {
        font-size: 13px;
    }

    .intro__decoration--one {
        left: 5%;
    }

    .intro__decoration--two {
        right: 4%;
    }
}
</style>