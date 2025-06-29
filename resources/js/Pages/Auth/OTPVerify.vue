<script setup>
import { ref, computed, onMounted } from "vue";
import { useForm } from "@inertiajs/vue3";

// Countdown Logic
const countdownDuration = 120;
const timeLeft = ref(countdownDuration);

const formattedTime = computed(() => {
    const minutes = Math.floor(timeLeft.value / 60);
    const seconds = timeLeft.value % 60;
    return `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
});

const startTimer = () => {
    const timer = setInterval(() => {
        if (timeLeft.value > 0) {
            timeLeft.value--;
        } else {
            clearInterval(timer);
        }
    }, 1000);
};
onMounted(startTimer);

// Form and OTP Logic
const source = sessionStorage.getItem('source');
const OTPForm = useForm({ otp: null });

const isOtpComplete = computed(() => OTPForm.otp && OTPForm.otp.length === 6);

const submitOtp = () => {
    if (isOtpComplete.value) {
        OTPForm.post(route(source), {
            onSuccess: () => {
                if (source.includes('resetpwd')) {
                    toast.success('Password reseted! Check your mail');
                } else {
                    toast.success('Registration Successful');
                }
            },
            onError: () => {
                toast.error(OTPForm.errors.message);
            },
        });
    }
};

// Optional resend OTP logic (if supported by your backend)
const resendOtp = () => {
    timeLeft.value = countdownDuration;
    startTimer();
    toast.info('OTP has been resent. Please check your email.');
};
</script>

<template>
    <div class="container d-flex justify-content-center align-items-center min-vh-100">
        <div class="p-4 px-10 py-10 text-center shadow card col-sm-12 col-md-4">
            <!-- Logo and Header -->
            <div class="mb-3">
                <img src="/public/images/freshcart-logo.svg" alt="Your Logo" class="mb-2 img-fluid">
                <p class="text-muted">Enter the OTP sent to your email</p>
            </div>

            <!-- OTP Input Form -->
            <form @submit.prevent="submitOtp">
                <div class="mb-3 d-flex justify-content-center">
                    <input
                        type="text"
                        maxlength="6"
                        class="text-center form-control fs-3"
                        placeholder="# # # # # #"
                        v-model="OTPForm.otp"
                    />
                </div>
                <button type="submit" class="btn main-theme w-100" :disabled="!isOtpComplete">
                    Verify Now
                </button>
            </form>

            <!-- Countdown Timer -->
            <div
                class="flex-col mt-3 rounded bg-secondary-subtle d-flex justify-content-center align-items-center"
                style="height: 100px;"
            >
                <p class="text-muted">Time remaining: {{ formattedTime }}</p>
                <p v-if="timeLeft === 0">
                    Didn’t receive the code?
                    <button type="button" class="btn btn-link text-success" @click="resendOtp">
                        Resend OTP
                    </button>
                </p>
            </div>
        </div>
    </div>
</template>
