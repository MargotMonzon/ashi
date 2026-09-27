<script setup>
import { ref } from 'vue'

const saidYes = ref(false)

const noButton = ref(null)

const noPosition = ref({
    x: 0,
    y: 0,
})

const escapeCount = ref(0)

const escapeButton = () => {
    if (saidYes.value) return

    const maxX = 130
    const maxY = 100

    const directionX = Math.random() > 0.5 ? 1 : -1
    const directionY = Math.random() > 0.5 ? 1 : -1

    const distanceX = 60 + Math.random() * maxX
    const distanceY = 30 + Math.random() * maxY

    noPosition.value = {
        x: directionX * distanceX,
        y: directionY * distanceY,
    }

    escapeCount.value++
}

const sayYes = () => {
    saidYes.value = true
}

const noButtonText = () => {
    if (escapeCount.value === 0) {
        return 'No'
    }

    if (escapeCount.value === 1) {
        return '¿Segura? 🤨'
    }

    if (escapeCount.value === 2) {
        return 'Piénsalo bien'
    }

    if (escapeCount.value === 3) {
        return 'JAJA no'
    }

    return 'Ni lo intentes 😌'
}
</script>

<template>
    <section id="pregunta" class="love-question">
        <!-- DECORACIONES -->

        <span class="
                love-question__decoration
                love-question__decoration--one
            ">
            ♡
        </span>

        <span class="
                love-question__decoration
                love-question__decoration--two
            ">
            ✦
        </span>

        <span class="
                love-question__decoration
                love-question__decoration--three
            ">
            ♡
        </span>

        <!-- PREGUNTA -->

        <div v-if="!saidYes" class="love-question__content">
            <span class="love-question__eyebrow">
                antes de seguir...
            </span>

            <span class="love-question__tiny">
                tengo una pregunta importante
            </span>

            <h2>
                ¿Todavía
                <span>me amas?</span>
            </h2>

            <p>
                Piénsalo cuidadosamente.
                Esta decisión podría cambiar
                el curso de la historia.
            </p>

            <div class="love-question__buttons">
                <button type="button" class="
                        love-question__button
                        love-question__button--yes
                    " @click="sayYes">
                    <span>
                        Sí
                    </span>

                    <span>
                        ♡
                    </span>
                </button>

                <button ref="noButton" type="button" class="
                        love-question__button
                        love-question__button--no
                    " :style="{
                        transform: `
                            translate(
                                ${noPosition.x}px,
                                ${noPosition.y}px
                            )
                        `,
                    }" @pointerenter="escapeButton" @touchstart.prevent="escapeButton" @click.prevent="escapeButton">
                    {{ noButtonText() }}
                </button>
            </div>

            <span v-if="escapeCount >= 2" class="love-question__warning">
                ese botón parece tener vida propia 🤨
            </span>
        </div>

        <!-- RESPUESTA SÍ -->

        <div v-else class="love-question__success">
            <div class="love-question__success-heart">
                ♥
            </div>

            <span class="love-question__eyebrow">
                sabía 😌
            </span>

            <h2>
                Yo también
                <span>te amo.</span>
            </h2>

            <div class="love-question__kiss">
                un bishito? ♡
            </div>

            <div class="love-question__continue">
                <span>
                    ahora sí, sigamos...
                </span>

                <span>
                    ↓
                </span>
            </div>

            <!-- CORAZONCITOS -->

            <div class="love-question__hearts">
                <span>♥</span>
                <span>♡</span>
                <span>♥</span>
                <span>♡</span>
                <span>♥</span>
                <span>♡</span>
                <span>♥</span>
                <span>♡</span>
            </div>
        </div>
    </section>
</template>

<style scoped>
.love-question {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    min-height: 100dvh;

    padding:
        120px 24px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 40%,
            rgba(255, 255, 255, 0.96),
            transparent 36%),
        linear-gradient(180deg,
            #fffaf8 0%,
            #fcecee 50%,
            #fffaf8 100%);
}

/* ============================= */
/* CONTENIDO */
/* ============================= */

.love-question__content,
.love-question__success {
    position: relative;

    z-index: 5;

    width: 100%;
    max-width: 700px;

    text-align: center;

    animation:
        questionAppear 1s ease both;
}

.love-question__eyebrow {
    display: inline-block;

    margin-bottom: 22px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.love-question__tiny {
    display: block;

    margin-bottom: 22px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 20px;
    font-style: italic;

    color: var(--text-soft);
}

.love-question h2 {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size:
        clamp(60px,
            12vw,
            105px);

    font-weight: 500;

    line-height: 0.83;
    letter-spacing: -0.04em;

    color: var(--wine);
}

.love-question h2 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

.love-question__content>p,
.love-question__success>p {
    max-width: 450px;

    margin:
        32px auto 0;

    font-size: 13px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

/* ============================= */
/* BOTONES */
/* ============================= */

.love-question__buttons {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    gap: 16px;

    min-height: 180px;

    margin-top: 25px;
}

.love-question__button {
    display: inline-flex;
    align-items: center;
    justify-content: center;

    gap: 10px;

    min-width: 140px;

    padding:
        15px 25px;

    border-radius: 999px;

    cursor: pointer;

    font-size: 13px;
    font-weight: 500;

    transition:
        transform 0.2s ease,
        background 0.25s ease,
        box-shadow 0.25s ease;
}

/* SÍ */

.love-question__button--yes {
    position: relative;

    z-index: 10;

    background:
        var(--wine);

    color: white;

    box-shadow:
        0 15px 40px rgba(107, 52, 62, 0.22);
}

.love-question__button--yes:hover {
    background:
        var(--red);

    transform:
        translateY(-3px) scale(1.04);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.28);
}

/* NO */

.love-question__button--no {
    position: relative;

    z-index: 9;

    border:
        1px solid rgba(169, 74, 90, 0.15);

    background:
        rgba(255, 255, 255, 0.65);

    color: var(--wine);

    box-shadow:
        0 10px 30px rgba(107, 52, 62, 0.06);

    backdrop-filter:
        blur(10px);

    transition:
        transform 0.28s cubic-bezier(0.2,
            0.8,
            0.2,
            1);
}

/* ============================= */
/* MENSAJITO */
/* ============================= */

.love-question__warning {
    display: block;

    margin-top: 12px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 17px;
    font-style: italic;

    color:
        rgba(107,
            52,
            62,
            0.5);

    animation:
        warningAppear 0.5s ease both;
}

/* ============================= */
/* RESPUESTA SÍ */
/* ============================= */

.love-question__success-heart {
    margin-bottom: 25px;

    font-size: 50px;

    color: var(--red);

    animation:
        bigHeartbeat 1.5s ease-in-out infinite;
}

.love-question__kiss {
    margin-top: 35px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 28px;
    font-style: italic;

    color: var(--red);
}

.love-question__continue {
    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 10px;

    margin-top: 65px;
}

.love-question__continue span:first-child {
    font-size: 9px;
    font-weight: 600;

    letter-spacing: 0.22em;
    text-transform: uppercase;

    color:
        rgba(107,
            52,
            62,
            0.4);
}

.love-question__continue span:last-child {
    font-size: 20px;

    color: var(--red);

    animation:
        arrowDown 1.8s ease-in-out infinite;
}

/* ============================= */
/* CORAZONES AL DECIR SÍ */
/* ============================= */

.love-question__hearts {
    position: absolute;

    inset: 0;

    pointer-events: none;

    overflow: visible;
}

.love-question__hearts span {
    position: absolute;

    bottom: 25%;

    font-size: 20px;

    color:
        rgba(169,
            74,
            90,
            0.55);

    animation:
        heartFly 4s ease-in infinite;
}

.love-question__hearts span:nth-child(1) {
    left: 10%;

    animation-delay: 0s;
}

.love-question__hearts span:nth-child(2) {
    left: 20%;

    animation-delay: -1.5s;
}

.love-question__hearts span:nth-child(3) {
    left: 32%;

    animation-delay: -0.6s;
}

.love-question__hearts span:nth-child(4) {
    left: 44%;

    animation-delay: -2.2s;
}

.love-question__hearts span:nth-child(5) {
    left: 58%;

    animation-delay: -1s;
}

.love-question__hearts span:nth-child(6) {
    left: 70%;

    animation-delay: -2.8s;
}

.love-question__hearts span:nth-child(7) {
    left: 82%;

    animation-delay: -0.3s;
}

.love-question__hearts span:nth-child(8) {
    left: 92%;

    animation-delay: -1.9s;
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.love-question__decoration {
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

.love-question__decoration--one {
    top: 15%;
    left: 5%;

    font-size: 100px;
}

.love-question__decoration--two {
    top: 30%;
    right: 10%;

    font-size: 32px;

    animation-delay: -2s;
}

.love-question__decoration--three {
    right: 3%;
    bottom: 12%;

    font-size: 120px;

    animation-delay: -4s;
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

@keyframes questionAppear {
    from {
        opacity: 0;

        transform:
            translateY(40px);
    }

    to {
        opacity: 1;

        transform:
            translateY(0);
    }
}

@keyframes warningAppear {
    from {
        opacity: 0;

        transform:
            translateY(8px);
    }

    to {
        opacity: 1;

        transform:
            translateY(0);
    }
}

@keyframes bigHeartbeat {

    0%,
    100% {
        transform:
            scale(1);
    }

    15% {
        transform:
            scale(1.2);
    }

    30% {
        transform:
            scale(1);
    }

    45% {
        transform:
            scale(1.12);
    }
}

@keyframes arrowDown {

    0%,
    100% {
        opacity: 0.3;

        transform:
            translateY(0);
    }

    50% {
        opacity: 1;

        transform:
            translateY(8px);
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
            translateY(-16px);
    }
}

@keyframes heartFly {
    0% {
        opacity: 0;

        transform:
            translateY(80px) scale(0.6) rotate(0deg);
    }

    20% {
        opacity: 1;
    }

    100% {
        opacity: 0;

        transform:
            translateY(-350px) scale(1.4) rotate(30deg);
    }
}

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 600px) {
    .love-question {
        padding:
            100px 20px;
    }

    .love-question__buttons {
        gap: 10px;

        min-height: 220px;
    }

    .love-question__button {
        min-width: 120px;

        padding:
            14px 20px;

        font-size: 12px;
    }

    .love-question__content>p,
    .love-question__success>p {
        max-width: 330px;

        font-size: 12px;
    }

    .love-question__decoration--one {
        left: -50px;
    }

    .love-question__decoration--three {
        right: -60px;
    }
}
</style>