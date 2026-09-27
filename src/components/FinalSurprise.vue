<script setup>
import {
    onBeforeUnmount,
    ref,
} from 'vue'

import { relationship } from '../data/relationship'

const revealed = ref(false)
const audioRef = ref(null)

const isPlaying = ref(false)

const revealSurprise = () => {
    revealed.value = true
}

const toggleMusic = async () => {
    if (!audioRef.value) {
        return
    }

    try {
        if (audioRef.value.paused) {
            await audioRef.value.play()

            isPlaying.value = true

            return
        }

        audioRef.value.pause()

        isPlaying.value = false
    } catch (error) {
        console.error(
            'No se pudo reproducir la canción:',
            error,
        )
    }
}

const handleSongEnded = () => {
    isPlaying.value = false
}

onBeforeUnmount(() => {
    if (!audioRef.value) {
        return
    }

    audioRef.value.pause()
})
</script>

<template>
    <section id="sorpresa-final" class="final-surprise" :class="{
        'final-surprise--revealed': revealed,
    }">
        <!-- AUDIO -->

        <audio ref="audioRef" :src="relationship.finalSurprise.song" preload="metadata"
            @ended="handleSongEnded"></audio>

        <!-- DECORACIONES -->

        <span class="
                final-surprise__decoration
                final-surprise__decoration--one
            ">
            ♡
        </span>

        <span class="
                final-surprise__decoration
                final-surprise__decoration--two
            ">
            ✦
        </span>

        <span class="
                final-surprise__decoration
                final-surprise__decoration--three
            ">
            ♡
        </span>

        <!-- ============================= -->
        <!-- ANTES DE REVELAR -->
        <!-- ============================= -->

        <div v-if="!revealed" class="final-surprise__locked">
            <span class="final-surprise__eyebrow">
                {{ relationship.finalSurprise.eyebrow }}
            </span>

            <div class="final-surprise__gift">
                <span>
                    ♡
                </span>
            </div>

            <h2>
                Llegaste
                <span>hasta aquí.</span>
            </h2>

            <p>
                Así que creo que ya puedes ver
                la última cosita que preparé para ti.
            </p>

            <button type="button" class="final-surprise__reveal" @click="revealSurprise">
                <span>
                    Ver última sorpresa
                </span>

                <span>
                    ♡
                </span>
            </button>
        </div>

        <!-- ============================= -->
        <!-- REVELADO -->
        <!-- ============================= -->

        <Transition name="reveal">
            <div v-if="revealed" class="final-surprise__content">
                <span class="final-surprise__eyebrow">
                    nosotros ♡
                </span>

                <h2>
                    {{ relationship.finalSurprise.title }}

                    <span>
                        {{
                            relationship
                                .finalSurprise
                                .titleAccent
                        }}
                    </span>
                </h2>

                <p class="final-surprise__message">
                    {{
                        relationship
                            .finalSurprise
                            .message
                    }}
                </p>

                <!-- FOTO -->

                <div class="final-surprise__photo-area">
                    <div class="final-surprise__photo-shadow"></div>

                    <div class="final-surprise__photo">
                        <img :src="relationship
                            .finalSurprise
                            .photo
                            " alt="Nosotros" />

                        <div class="final-surprise__photo-bottom">
                            <span>
                                {{
                                    relationship
                                        .finalSurprise
                                        .date
                                }}
                            </span>

                            <span>
                                ♡
                            </span>
                        </div>
                    </div>

                    <span class="
                            final-surprise__tape
                            final-surprise__tape--left
                        "></span>

                    <span class="
                            final-surprise__tape
                            final-surprise__tape--right
                        "></span>
                </div>

                <!-- CANCIÓN -->

                <div class="final-surprise__music">
                    <div class="final-surprise__music-info">
                        <span class="final-surprise__music-icon">
                            ♪
                        </span>

                        <div>
                            <span>
                                nuestra canción
                            </span>

                            <strong>
                                {{
                                    relationship
                                        .finalSurprise
                                        .songName
                                }}
                            </strong>
                        </div>
                    </div>

                    <button type="button" class="final-surprise__play" :class="{
                        'final-surprise__play--active':
                            isPlaying,
                    }" @click="toggleMusic">
                        <span v-if="!isPlaying">
                            ▶
                        </span>

                        <span v-else>
                            Ⅱ
                        </span>
                    </button>

                    <div v-if="isPlaying" class="final-surprise__waves">
                        <span></span>
                        <span></span>
                        <span></span>
                        <span></span>
                        <span></span>
                    </div>
                </div>

                <div class="final-surprise__bias">
                    <span class="final-surprise__bias-eyebrow">
                        invitado especial xd
                    </span>

                    <div class="final-surprise__bias-card">
                        <img :src="relationship.finalSurprise.biasPhoto" alt="Imagen especial" />

                        <p>
                            {{ relationship.finalSurprise.biasMessage }}
                        </p>
                    </div>
                </div>

                <!-- FRASE FINAL -->

                <div class="final-surprise__ending">
                    <span class="final-surprise__ending-heart">
                        ♥
                    </span>

                    <p>
                        {{
                            relationship
                                .finalSurprise
                                .finalMessage
                        }}
                    </p>

                    <div class="final-surprise__names">
                        <span>
                            {{ relationship.yourName }}
                        </span>

                        <span>
                            +
                        </span>

                        <span>
                            {{ relationship.herName }}
                        </span>
                    </div>

                    <span class="final-surprise__date">
                        {{
                            relationship
                                .finalSurprise
                                .date
                        }}
                        — ∞
                    </span>
                </div>

                <div class="final-surprise__footer">
                    <span>
                        hecho con amor
                    </span>

                    <span>
                        ♡
                    </span>

                    <span>
                        para ti
                    </span>
                </div>

                <!-- CORAZONES -->

                <div class="final-surprise__floating-hearts">
                    <span>♡</span>
                    <span>♥</span>
                    <span>♡</span>
                    <span>♥</span>
                    <span>♡</span>
                    <span>♥</span>
                </div>
            </div>
        </Transition>
    </section>
</template>

<style scoped>
.final-surprise__bias {
    margin-top: 55px;

    text-align: center;
}

.final-surprise__bias-eyebrow {
    display: inline-block;

    margin-bottom: 18px;

    font-size: 9px;
    font-weight: 600;

    letter-spacing: 0.18em;
    text-transform: uppercase;

    color: rgba(107, 52, 62, 0.45);
}

.final-surprise__bias-card {
    width: 100%;
    max-width: 320px;

    margin: 0 auto;

    padding: 14px 14px 20px;

    border: 1px solid rgba(169, 74, 90, 0.1);
    border-radius: 28px 8px 28px 8px;

    background: rgba(255, 255, 255, 0.58);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.08);

    backdrop-filter: blur(12px);
}

.final-surprise__bias-card img {
    width: 100%;

    border-radius: 18px;

    display: block;
}

.final-surprise__bias-card p {
    margin-top: 14px;

    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-style: italic;

    color: var(--wine);
}

.final-surprise {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    min-height: 100dvh;

    padding:
        130px 24px 100px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 20%,
            rgba(255, 255, 255, 0.98),
            transparent 30%),
        linear-gradient(180deg,
            #fffaf8 0%,
            #f8e3e6 52%,
            #f4d9dc 100%);
}

/* ============================= */
/* GENERAL */
/* ============================= */

.final-surprise__locked,
.final-surprise__content {
    position: relative;

    z-index: 5;

    width: 100%;
    max-width: 800px;

    text-align: center;
}

.final-surprise__eyebrow {
    display: inline-block;

    margin-bottom: 25px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.final-surprise h2 {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size:
        clamp(56px,
            11vw,
            96px);

    font-weight: 500;

    line-height: 0.87;
    letter-spacing: -0.04em;

    color: var(--wine);
}

.final-surprise h2 span {
    display: block;

    font-style: italic;

    color: var(--red);
}

/* ============================= */
/* BLOQUE INICIAL */
/* ============================= */

.final-surprise__locked {
    animation:
        appear 1s ease both;
}

.final-surprise__gift {
    display: flex;
    align-items: center;
    justify-content: center;

    width: 88px;
    height: 88px;

    margin:
        0 auto 28px;

    border:
        1px solid rgba(169, 74, 90, 0.14);

    border-radius: 50%;

    background:
        rgba(255, 255, 255, 0.5);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.08);

    backdrop-filter:
        blur(10px);

    animation:
        giftFloat 2.8s ease-in-out infinite;
}

.final-surprise__gift span {
    font-size: 36px;

    color: var(--red);

    animation:
        heartbeat 2s ease-in-out infinite;
}

.final-surprise__locked>p {
    max-width: 430px;

    margin:
        30px auto 0;

    font-size: 13px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

.final-surprise__reveal {
    display: inline-flex;
    align-items: center;
    justify-content: center;

    gap: 12px;

    margin-top: 35px;

    padding:
        16px 28px;

    border: none;
    border-radius: 999px;

    background: var(--wine);

    color: white;

    font-size: 13px;
    font-weight: 500;

    cursor: pointer;

    box-shadow:
        0 16px 45px rgba(107, 52, 62, 0.22);

    transition:
        transform 0.25s ease,
        background 0.25s ease,
        box-shadow 0.25s ease;
}

.final-surprise__reveal:hover {
    background: var(--red);

    transform:
        translateY(-3px);

    box-shadow:
        0 22px 55px rgba(107, 52, 62, 0.28);
}

/* ============================= */
/* CONTENIDO REVELADO */
/* ============================= */

.final-surprise__content {
    padding-bottom: 30px;
}

.final-surprise__message {
    max-width: 500px;

    margin:
        32px auto 0;

    font-size: 14px;
    font-weight: 300;

    line-height: 1.9;

    color: var(--text-soft);
}

/* ============================= */
/* FOTO */
/* ============================= */

.final-surprise__photo-area {
    position: relative;

    width: fit-content;

    margin:
        80px auto 70px;
}

.final-surprise__photo-shadow {
    position: absolute;

    inset:
        20px -20px -20px 20px;

    border-radius: 8px;

    background:
        rgba(107, 52, 62, 0.08);

    filter:
        blur(15px);
}

.final-surprise__photo {
    position: relative;

    width: 390px;

    padding:
        14px 14px 18px;

    background: white;

    box-shadow:
        0 30px 80px rgba(107, 52, 62, 0.16);

    transform:
        rotate(-2deg);

    transition:
        transform 0.4s ease;
}

.final-surprise__photo:hover {
    transform:
        rotate(0deg) scale(1.015);
}

.final-surprise__photo img {
    width: 100%;
    height: 460px;

    object-fit: cover;
}

.final-surprise__photo-bottom {
    display: flex;
    align-items: center;
    justify-content: space-between;

    padding:
        17px 8px 3px;

    font-family:
        'Cormorant Garamond',
        serif;

    color: var(--wine);
}

.final-surprise__photo-bottom span:first-child {
    font-size: 18px;
    font-style: italic;
}

.final-surprise__photo-bottom span:last-child {
    font-size: 23px;

    color: var(--red);
}

/* ============================= */
/* CINTA FOTO */
/* ============================= */

.final-surprise__tape {
    position: absolute;

    z-index: 3;

    width: 80px;
    height: 25px;

    background:
        rgba(244, 217, 220, 0.72);

    backdrop-filter:
        blur(4px);
}

.final-surprise__tape--left {
    top: -8px;
    left: -28px;

    transform:
        rotate(-35deg);
}

.final-surprise__tape--right {
    top: -10px;
    right: -30px;

    transform:
        rotate(34deg);
}

/* ============================= */
/* MÚSICA */
/* ============================= */

.final-surprise__music {
    position: relative;

    display: flex;
    align-items: center;

    gap: 18px;

    width: 100%;
    max-width: 470px;

    margin:
        0 auto;

    padding:
        16px 18px;

    border:
        1px solid rgba(169, 74, 90, 0.12);

    border-radius: 22px;

    background:
        rgba(255, 255, 255, 0.58);

    box-shadow:
        0 20px 50px rgba(107, 52, 62, 0.07);

    backdrop-filter:
        blur(12px);
}

.final-surprise__music-info {
    display: flex;
    align-items: center;

    gap: 14px;

    flex: 1;

    text-align: left;
}

.final-surprise__music-icon {
    display: flex;
    align-items: center;
    justify-content: center;

    width: 42px;
    height: 42px;

    border-radius: 50%;

    background:
        var(--pink-light);

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 23px;

    color: var(--red);
}

.final-surprise__music-info div {
    display: flex;
    flex-direction: column;

    gap: 3px;
}

.final-surprise__music-info div>span {
    font-size: 8px;
    font-weight: 600;

    letter-spacing: 0.17em;
    text-transform: uppercase;

    color:
        rgba(107, 52, 62, 0.45);
}

.final-surprise__music-info strong {
    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 18px;
    font-weight: 500;

    color: var(--wine);
}

.final-surprise__play {
    display: flex;
    align-items: center;
    justify-content: center;

    width: 46px;
    height: 46px;

    flex-shrink: 0;

    border: none;
    border-radius: 50%;

    background: var(--wine);

    color: white;

    cursor: pointer;

    transition:
        transform 0.25s ease,
        background 0.25s ease;
}

.final-surprise__play:hover {
    background: var(--red);

    transform:
        scale(1.06);
}

.final-surprise__play--active {
    animation:
        musicPulse 1.8s ease-in-out infinite;
}

/* ONDITAS */

.final-surprise__waves {
    position: absolute;

    right: 78px;
    bottom: 13px;

    display: flex;
    align-items: flex-end;

    gap: 3px;

    height: 15px;
}

.final-surprise__waves span {
    width: 2px;

    border-radius: 999px;

    background: var(--red);

    animation:
        wave 0.8s ease-in-out infinite alternate;
}

.final-surprise__waves span:nth-child(1) {
    height: 5px;
}

.final-surprise__waves span:nth-child(2) {
    height: 11px;

    animation-delay: -0.3s;
}

.final-surprise__waves span:nth-child(3) {
    height: 7px;

    animation-delay: -0.15s;
}

.final-surprise__waves span:nth-child(4) {
    height: 14px;

    animation-delay: -0.45s;
}

.final-surprise__waves span:nth-child(5) {
    height: 8px;

    animation-delay: -0.25s;
}

/* ============================= */
/* FINAL */
/* ============================= */

.final-surprise__ending {
    max-width: 600px;

    margin:
        100px auto 0;

    padding:
        60px 30px;

    border:
        1px solid rgba(169, 74, 90, 0.1);

    border-radius:
        50px 8px 50px 8px;

    background:
        rgba(255, 255, 255, 0.48);

    box-shadow:
        0 30px 80px rgba(107, 52, 62, 0.08);

    backdrop-filter:
        blur(12px);
}

.final-surprise__ending-heart {
    display: block;

    margin-bottom: 25px;

    font-size: 34px;

    color: var(--red);

    animation:
        heartbeat 2s ease-in-out infinite;
}

.final-surprise__ending p {
    max-width: 470px;

    margin:
        0 auto;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size:
        clamp(24px,
            5vw,
            34px);

    font-style: italic;

    line-height: 1.4;

    color: var(--wine);
}

.final-surprise__names {
    display: flex;
    align-items: center;
    justify-content: center;

    gap: 12px;

    margin-top: 35px;

    font-family:
        'Cormorant Garamond',
        serif;

    font-size: 23px;

    color: var(--red);
}

.final-surprise__names span:nth-child(2) {
    font-size: 15px;

    color:
        rgba(107, 52, 62, 0.4);
}

.final-surprise__date {
    display: block;

    margin-top: 10px;

    font-size: 9px;
    font-weight: 600;

    letter-spacing: 0.2em;

    color:
        rgba(107, 52, 62, 0.45);
}

/* ============================= */
/* FOOTER */
/* ============================= */

.final-surprise__footer {
    display: flex;
    align-items: center;
    justify-content: center;

    gap: 10px;

    margin-top: 70px;

    font-size: 8px;
    font-weight: 600;

    letter-spacing: 0.18em;
    text-transform: uppercase;

    color:
        rgba(107, 52, 62, 0.35);
}

.final-surprise__footer span:nth-child(2) {
    font-size: 15px;

    color: var(--red);
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.final-surprise__decoration {
    position: absolute;

    user-select: none;
    pointer-events: none;

    color:
        rgba(169, 74, 90, 0.08);

    animation:
        floating 7s ease-in-out infinite;
}

.final-surprise__decoration--one {
    top: 12%;
    left: 4%;

    font-size: 110px;
}

.final-surprise__decoration--two {
    top: 42%;
    right: 7%;

    font-size: 35px;

    animation-delay: -2s;
}

.final-surprise__decoration--three {
    right: 1%;
    bottom: 8%;

    font-size: 140px;

    animation-delay: -4s;
}

/* ============================= */
/* CORAZONES FINALES */
/* ============================= */

.final-surprise__floating-hearts {
    position: absolute;

    inset: 0;

    pointer-events: none;
}

.final-surprise__floating-hearts span {
    position: absolute;

    bottom: 0;

    color:
        rgba(169, 74, 90, 0.3);

    animation:
        heartRise 7s linear infinite;
}

.final-surprise__floating-hearts span:nth-child(1) {
    left: 8%;

    animation-delay: -1s;
}

.final-surprise__floating-hearts span:nth-child(2) {
    left: 23%;

    animation-delay: -5s;
}

.final-surprise__floating-hearts span:nth-child(3) {
    left: 41%;

    animation-delay: -3s;
}

.final-surprise__floating-hearts span:nth-child(4) {
    left: 62%;

    animation-delay: -6s;
}

.final-surprise__floating-hearts span:nth-child(5) {
    left: 78%;

    animation-delay: -2s;
}

.final-surprise__floating-hearts span:nth-child(6) {
    left: 91%;

    animation-delay: -4s;
}

/* ============================= */
/* TRANSICIÓN */
/* ============================= */

.reveal-enter-active {
    transition:
        opacity 1.1s ease,
        transform 1.1s ease;
}

.reveal-enter-from {
    opacity: 0;

    transform:
        translateY(50px);
}

.reveal-enter-to {
    opacity: 1;

    transform:
        translateY(0);
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

@keyframes appear {
    from {
        opacity: 0;

        transform:
            translateY(35px);
    }

    to {
        opacity: 1;

        transform:
            translateY(0);
    }
}

@keyframes giftFloat {

    0%,
    100% {
        transform:
            translateY(0);
    }

    50% {
        transform:
            translateY(-8px);
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
            scale(1.16);
    }

    30% {
        transform:
            scale(1);
    }
}

@keyframes musicPulse {

    0%,
    100% {
        box-shadow:
            0 0 0 0 rgba(169, 74, 90, 0.25);
    }

    50% {
        box-shadow:
            0 0 0 10px rgba(169, 74, 90, 0);
    }
}

@keyframes wave {
    from {
        transform:
            scaleY(0.5);
    }

    to {
        transform:
            scaleY(1.15);
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

@keyframes heartRise {
    0% {
        opacity: 0;

        transform:
            translateY(60px) scale(0.7);
    }

    15% {
        opacity: 1;
    }

    100% {
        opacity: 0;

        transform:
            translateY(-700px) scale(1.3);
    }
}

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 600px) {
    .final-surprise {
        padding:
            100px 20px 80px;
    }

    .final-surprise__photo-area {
        margin:
            65px auto 55px;
    }

    .final-surprise__photo {
        width:
            min(82vw,
                340px);
    }

    .final-surprise__photo img {
        height: 390px;
    }

    .final-surprise__music {
        max-width: 350px;

        padding:
            14px;
    }

    .final-surprise__music-info strong {
        font-size: 16px;
    }

    .final-surprise__ending {
        margin-top: 75px;

        padding:
            50px 22px;
    }

    .final-surprise__decoration--one {
        left: -55px;
    }

    .final-surprise__decoration--three {
        right: -75px;
    }
}
</style>