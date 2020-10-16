<template>
<div>
    <form @submit.prevent="userRegister" novalidate>
        <div class="login-wrap">
            <div class="login-html">
                <div class="login-form">
                    

                    <!--Первый этап -->
                    <transition name="slide-fade">
                        <div class="dForm" v-show="step === 1">
                        <h2>Регистрация</h2>
                            <div class="group">
                                <label class="label" for="name">*Ваше имя</label>

                                <input @blur="$v.formReg.name.$touch()"
                                        :class="status($v.formReg.name)"
                                        v-model.trim="formReg.name"
                                        type="text" class="input" id="name">

                                <div class="error" v-if="!$v.formReg.name.required">{{ reqText }}</div>
                                <div class="error" v-if="!$v.formReg.name.alpha">{{ alphaText }}</div>
                            </div>

                            <div class="group">
                                <label class="label" for="surname">*Ваша фамилия</label>

                                <input  @blur="$v.formReg.surname.$touch()"
                                        :class="status($v.formReg.surname)"
                                        v-model.trim="formReg.surname"
                                        type="text" class="input" id="surname">

                                <div class="error" v-if="!$v.formReg.surname.required">{{ reqText }}</div>
                                <div class="error" v-if="!$v.formReg.surname.alpha">{{ alphaText }}</div>
                            </div>

                            <div class="group">
                                <label class="label" for="surname">Ваше отчество</label>

                                <input @blur="$v.formReg.fathername.$touch()"
                                        :class="status($v.formReg.fathername)"
                                        v-model.trim="formReg.fathername"
                                        type="text" class="input" id="fathername">
                            </div>

                            <div class="group drop">
                                <div >
                                    <label class="label">*Дата рождения</label>
                                    <input type="date" class="input" v-model.trim="$v.formReg.date.$model">
                                </div>
                                <div class="error" v-if="!$v.formReg.date.required">{{reqText}}</div>    
                            </div>
                            <div class="group">
                                <div>
                                    <label class="label">*Номер телефона</label>
                                    <input type="text" class="input" v-model.trim="$v.formReg.phone.$model">
                                </div>
                                <div class="error" v-if="!$v.formReg.phone.required">{{reqText}}</div>
                                <div class="error" v-if="!$v.formReg.phone.integer">{{reqInt}}</div>
                                <div class="error" v-if="!$v.formReg.phone.strongPhone">{{phoneLength}}</div>
                                <div class="error" v-if="!$v.formReg.phone.start7">{{start}}</div>
                            </div>

                            <div class="row">
                                <label class="label">Пол</label><br><br>
                                <div class="radBlock">
                                    <label class="input">
                                        <input class="input" type="radio" value="male" v-model.trim="$v.formReg.gender.$model">Муж
                                    </label>
                                </div>
                                <div class="radBlock">
                                    <label class="input">
                                        <input class="input" type="radio" value="female" v-model.trim="$v.formReg.gender.$model">Жен
                                    </label>
                                </div>
                            </div>

                            <div class="group drop">
                                <label class="label">*Группа клиентов</label>
                                <select class="input multi" v-model.trim="$v.formReg.clients.$model">
                                    <option class="option" value="VIP">VIP</option>
                                    <option class="option" value="Проблемные">Проблемные</option>
                                    <option class="option" value="ОМС">ОМС</option>
                                </select>
                                <div class="error" v-if="!$v.formReg.clients.required">{{reqText}}</div>
                            </div>

                            <div class="row">
                                <label class="label">Лечащий врач</label><br><br>
                                <div class="radBlock">
                                    <label class="input">
                                        <input class="input" type="radio" value="Иванов" v-model.trim="$v.formReg.doctor.$model">Иванов
                                    </label>
                                </div>
                                <div class="radBlock">
                                    <label class="input">
                                        <input class="input" type="radio" value="Захаров" v-model.trim="$v.formReg.doctor.$model">Захаров
                                    </label>
                                </div>
                                <div class="radBlock">
                                    <label class="input">
                                        <input class="input" type="radio" value="Чернышева" v-model.trim="$v.formReg.doctor.$model">Чернышева
                                    </label>
                                </div>
                            </div>

                            <label class="row">
                                <input class="input" type="checkbox" name="sms-agreement" v-model.trim="$v.formReg.sms.$model">
                                <span class="label">Не отправлять мне смс.</span>
                            </label>
                            <div class="group">
                            <button @click="step++" :disabled="disabledBtn1"
                                    type="button" class="button">Далее</button>
                            </div>

                        </div>
                    </transition>
        

                    <!--Второй этап -->
                    <transition name="slide-fade">
                        <div class="dForm" v-show="step === 2">
                        <h2>Адрес</h2>
                            <div class="group">
                                <label class="label" for="index">Индекс</label>

                                <input v-model.trim="formReg.index" type="text" class="input" id="index">

                            </div>

                            <div class="group">
                                <label class="label" for="country">Страна</label>

                                <input v-model.trim="formReg.country" type="text" class="input" id="country">

                            </div>

                            <div class="group">
                                <label class="label" for="region">Регион</label>

                                <input v-model.trim="formReg.region" type="text" class="input" id="region">

                            </div>

                            <div class="group">
                                <label class="label" for="city">Город</label>

                                <input @blur="$v.formReg.city.$touch()"
                                        :class="status($v.formReg.city)"
                                        v-model.trim="formReg.city"
                                        type="text" class="input" id="city">

                                <div class="error" v-if="!$v.formReg.city.alpha">{{ alphaText }}</div>
                                <div class="error" v-if="!$v.formReg.city.required">{{reqText}}</div>
                            </div>

                            <div class="group">
                                <label class="label" for="street">Улица</label>

                                <input v-model.trim="formReg.street" type="text" class="input" id="street">

                            </div>

                            <div class="group">
                                <label class="label" for="house">Дом</label>

                                <input v-model.trim="formReg.house" type="text" class="input" id="house">

                            </div>
                            <div class="row">
                                <button @click="step--" type="button" class="button">Назад</button>
                                <button @click="step++" :disabled="disabledBtn2"
                                        type="button" class="button disbtn">Далее</button>
                            </div>

                        </div>
                    </transition>
            
                    <!--Третий этап -->

                    <transition name="slide-fade">
                        <div class="dForm" v-show="step === 3">
                        <h2>Документ</h2>
                            <div class="row">
                                <label class="label">*Тип документа</label><br><br>
                                <div class="radBlock">
                                    <label class="label">
                                        <input class="input" type="radio" value="Пасспорт" v-model.trim="$v.formReg.document.$model">Пасспорт
                                    </label>
                                </div>
                                <div class="radBlock">
                                    <label class="label">
                                        <input class="input" type="radio" value="Свидетельство о рождении" v-model.trim="$v.formReg.document.$model">Свидетельство о рождении
                                    </label>
                                </div>
                                <div class="radBlock">
                                    <label class="label">
                                        <input class="input" type="radio" value="Вод. удостоверение" v-model.trim="$v.formReg.document.$model">Вод. удостоверение
                                    </label>
                                </div>

                                <div class="error" v-if="!$v.formReg.document.required">{{reqText}}</div>
                            </div>

                            <div class="group">
                                <label class="label" for="series">Серия</label>

                                <input v-model.trim="formReg.series" type="text" class="input" id="series">

                            </div>

                            <div class="group">
                                <label class="label" for="number">Номер</label>

                                <input v-model.trim="formReg.number" type="text" class="input" id="number">

                            </div>

                            <div class="group">
                                <label class="label" for="issuedBy">Кем выдан</label>

                                <input v-model.trim="formReg.issuedBy" type="text" class="input" id="issuedBy">

                            </div>

                            <div class="group drop">
                                <div >
                                    <label class="label">*Дата выдачи</label>
                                    <input type="date" class="input" v-model.trim="$v.formReg.dateOfIssue.$model">
                                </div>
                                <div class="error" v-if="!$v.formReg.dateOfIssue.required">{{reqText}}</div>    
                            </div>

                            
                            <div class="row">    
                                <button @click="step--" type="button" class="button">Назад</button>
                                <button @click="step++" :disabled="disabledBtnFinish" type="submit" class="button disbtn">Готово</button>
                            </div>

                        </div>

                    </transition>
        
            

                    <!--Регестрация закончина -->
                    
                    <div class="dForm" v-show="step === 4">

                    
                        <div class="group">
                            <h2>Спасибо за регестрацию!</h2>
                            <div success>
                                <img src="../assets/registered.png" alt="Succes" width="100%">
                            </div>
                        </div>
                        

                    </div> 
                </div>
            </div>
        </div>
        
    </form>
    <footer>
        <div>
            <p>Created by <a href="https://rustam-raybik.netlify.app/">Rustam Raybik</a></p>
        </div>
    </footer>
</div>
</template>

<script>
import { required, integer, helpers } from 'vuelidate/lib/validators'
const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/);
const start7 = helpers.regex('start7', /^7[^\s]*$/i);
export default {
data() {
    return {
        step: 1,
        regMessage: false,
        reqText: 'Поле обязательно для заполнения',
        alphaText: 'Запрещены цифры, пробелы и другие символы',
        phoneLength: 'Номер телефона должен содержать 11 цифр',
        reqInt: 'Номер телефона должен содержать только цифры',
        start: 'Номер телефона должен начинаться с цифры 7',
        formReg: {
        name: '',
        surname: '',
        fathername: '',
        date: '',
        phone: '',
        gender: '',
        clients: '',
        doctor: '',
        sms: '',
        index: '',
        country: '',
        region: '',
        city: '',
        street: '',
        house: '',
        document: '',
        series: '',
        number: '',
        issuedBy: '',
        dateOfIssue: ''
        }
    }
},
computed: {
    disabledBtn1() {
    return this.$v.formReg.name.$invalid || 
            this.$v.formReg.surname.$invalid ||
            this.$v.formReg.date.$invalid ||
            this.$v.formReg.phone.$invalid ||
            this.$v.formReg.clients.$invalid
    },
    disabledBtn2() {
    return this.$v.formReg.city.$invalid
    },
    disabledBtnFinish() {
    return this.$v.formReg.document.$invalid ||
            this.$v.formReg.dateOfIssue.$invalid
    }
},
methods: {
    status(validation) {
    return {
        'is-invalid': validation.$error
    }
    },
    userRegister() {
        console.group()
        console.log('Вы успешно зарегистрированны!')
        console.log('Имя: ' + this.formReg.name)
        console.log('Фамилия: ' + this.formReg.surname)
        console.log('Отчество: ' + this.formReg.fathername)
        console.log('Дата рождения: ' + this.formReg.date)
        console.log('Номер телефона: ' + this.formReg.phone)
        console.log('Пол: ' + this.formReg.gender)
        console.log('Группа клиентов: ' + this.formReg.clients)
        console.log('Лечащий врач: ' + this.formReg.doctor)
        console.log('Не отправлять смс: ' + this.formReg.sms)
        console.log('Индекс: ' + this.formReg.index)
        console.log('Страна: ' + this.formReg.country)
        console.log('Область: ' + this.formReg.region)
        console.log('Город: ' + this.formReg.city)
        console.log('Улица: ' + this.formReg.street)
        console.log('Дом: ' + this.formReg.house)
        console.log('Тип документа: ' + this.formReg.document)
        console.log('Серия: ' + this.formReg.series)
        console.log('Номер: ' + this.formReg.number)
        console.log('Кем выдан: ' + this.formReg.issuedBy)
        console.log('Дата выдачи: ' + this.formReg.dateOfIssue)
        console.groupEnd()
        
        
    }
},
validations: {
    formReg: {
        name: {
            required,
            alpha
        },
        surname: {
            required,
            alpha
        },
        fathername: {

        },
        date: {
            required
        },
        phone: {
            required,
            integer,
            strongPhone(value){
                return value.trim().length === 11
            },
            start7
        },
        gender: {

        },
        clients:{
            required
        },
        doctor: {

        },
        sms: {

        },
        index: {

        },
        country: {
            alpha
        },
        region: {

        },
        city: {
            alpha,
            required
        },
        street: {

        },
        house: {

        },
        document: {
            required
        },
        series: {

        },
        number: {

        },
        issuedBy: {

        },
        dateOfIssue: {
            required
        }
    }
}
}
</script>

<style>
.slide-fade-enter-active {
transition: all 0.6s ease;
}
.slide-fade-enter {
transform: translateX(20px);
opacity: 0;
}
</style>