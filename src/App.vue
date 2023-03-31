<script>
import axios from 'axios';

export default {
    data() {
        return {
            city: '',
            error: '',
            info: null,
        };
    },
    computed: {
        cityName() {
            return '«' + this.city + '»';
        },
        showTemp() {
            return 'Температура: ' + this.info.main.temp + ' °C';
        },
        showFellsLike() {
            return 'Ощущается как: ' + this.info.main.feels_like + ' °C';
        },
        showMinTemp() {
            return (
                'Минимальная температура: ' + this.info.main.temp_min + ' °C'
            );
        },
        showMaxTemp() {
            return (
                'Максимальная температура: ' + this.info.main.temp_max + ' °C'
            );
        },
        showPressure() {
            return 'Давление: ' + this.info.main.pressure;
        },
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 2) {
                this.error = 'Напишите полное название города!';
                return false;
            }
            axios
                .get(
                    `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=9c35213542b7f002aed8947c45620336`
                )
                .then((res) => {
                    this.info = res.data;
                });
        },
    },
};
</script>

<template>
    <div class="wrapper">
        <h1>Погода</h1>
        <p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
        <input type="text" v-model="city" placeholder="Введите город" />

        <button v-if="city != ''" @click="getWeather()">Узнать погоду</button>
        <button disabled v-else>Введите название города</button>
        <p class="error">{{ error }}</p>
        <div v-if="info != null">
            <p>{{ showTemp }}</p>
            <p>{{ showFellsLike }}</p>
            <p>{{ showMaxTemp }}</p>
            <p>{{ showMinTemp }}</p>
            <p>{{ showPressure }}</p>
        </div>
    </div>
</template>

<style scoped>
.error {
    color: red;
}
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background-color: #6a9fbb;
    text-align: center;
    padding: 20px;
}

.wrapper h1 {
    margin-top: 50px;
}

.wrapper p {
    margin-top: 15px;
}

.wrapper input {
    margin-top: 30px;
    background-color: transparent;
    border: 0;
    border-bottom: 2px solid #000;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

.wrapper input:focus {
    border-bottom-color: #6e2d7d;
}

.wrapper button {
    background: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    margin-left: 20px;
    padding: 10px 15px;
    cursor: pointer;
    transition: transform 500ms ease;
}

.wrapper button:disabled {
    background-color: gray;
    border: 2px solid gray;
    transform: none;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}
</style>
