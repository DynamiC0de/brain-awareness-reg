<template>
    <div>
        <h2>Regisztráció</h2>
        <form @submit.prevent="handleSubmit">
            <div class="form-group">
                <label for="name">Teljes Név</label>
                <input type="text" v-model="user.name" v-validate="'required'" name="firstName" class="form-control" :class="{ 'is-invalid': submitted && errors.has('name') }" />
                <div v-if="submitted && errors.has('name')" class="invalid-feedback">{{ errors.first('name') }}</div>
            </div>
            <div class="form-group">
                <label for="email">E-Mail cím</label>
                <input type="email" v-model="user.email" v-validate="'required'" name="email" class="form-control" :class="{ 'is-invalid': submitted && errors.has('email') }" />
                <div v-if="submitted && errors.has('email')" class="invalid-feedback">{{ errors.first('email') }}</div>
            </div>
            <div class="form-group">
              <label for="tel">Telefonszám</label>
              <input type="tel" v-model="user.tel" v-validate="'digits:11'" name="tel" class="form-control" :class="{ 'is-invalid': submitted && errors.has('tel') }" />
              <div v-if="submitted && errors.has('tel')" class="invalid-feedback">{{ errors.first('tel') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="age">Életkor</label>
                <input type="number" v-model="user.age" v-validate="'between:1,99'" name="age" class="form-control" :class="{ 'is-invalid': submitted && errors.has('age') }" />
                <div v-if="submitted && errors.has('age')" class="invalid-feedback">{{ errors.first('age') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="school">Melyik iskolából jelentkezel?</label>
                <input type="text" v-model="user.school" v-validate="'required'" name="school" class="form-control" :class="{ 'is-invalid': submitted && errors.has('school') }" />
                <div v-if="submitted && errors.has('school')" class="invalid-feedback">{{ errors.first('school') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="class">Hányadik osztályos vagy?</label>
                <input type="number" v-model="user.class" v-validate="'between:0,12'" name="class" class="form-control" :class="{ 'is-invalid': submitted && errors.has('class') }" />
                <div v-if="submitted && errors.has('class')" class="invalid-feedback">{{ errors.first('class') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="teamName">Mi a csapatotok neve?</label>
                <input type="text" v-model="user.teamName" v-validate="'required'" name="teamName" class="form-control" :class="{ 'is-invalid': submitted && errors.has('teamName') }" />
                <div v-if="submitted && errors.has('teamName')" class="invalid-feedback">{{ errors.first('teamName') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="teamMembers">Kik a csapattagok? (max. 4 fő/csapat)</label>
                <input type="text" v-model="user.teamMembers" v-validate="'required'" name="teamMembers" class="form-control" :class="{ 'is-invalid': submitted && errors.has('teamMembers') }" />
                <div v-if="submitted && errors.has('teamMembers')" class="invalid-feedback">{{ errors.first('teamMembers') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="teamDescription">Mutatkozzatok be 4-5 mondatban!</label>
                <input type="text" v-model="user.teamName" v-validate="'required'" name="teamName" class="form-control" :class="{ 'is-invalid': submitted && errors.has('teamName') }" />
                <div v-if="submitted && errors.has('teamName')" class="invalid-feedback">{{ errors.first('teamName') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="statementFile">Hozzájáruló nyilatkozat feltöltése (mind a 4 csapattagé egy fájlban legyen)
                <br>
                <a href="src/HOZZÁJÁRULÓ_NYILATKOZAT.pdf">Letöltés itt</a>
                </label>
                <input type="file" v-on="change" v-validate="'required'" name="statementFile" class="form-control" :class="{ 'is-invalid': submitted && errors.has('statementFile') }" />
                <div v-if="submitted && errors.has('statementFile')" class="invalid-feedback">{{ errors.first('statementFile') }}</div>
            </div>
            <div class="form-group">
                <label htmlFor="privacyAccept">Az adatvédelmi tájékoztatót elolvastam, megértettem és az abban foglaltakat elfogadom!
                <br>
                <a href="src/Adatvédelmi_tájékoztató_versenyző.pdf">Letöltés itt</a>
                </label>
                <input type="checkbox" v-model="user.privacyAccept" v-validate="'required'" name="privacyAccept" class="form-control" :class="{ 'is-invalid': submitted && errors.has('privacyAccept') }" />
                <div v-if="submitted && errors.has('privacyAccept')" class="invalid-feedback">{{ errors.first('privacyAccept') }}</div>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" :disabled="status.registering">Regisztráció</button>
                <img v-show="status.registering" src="data:image/gif;base64,R0lGODlhEAAQAPIAAP///wAAAMLCwkJCQgAAAGJiYoKCgpKSkiH/C05FVFNDQVBFMi4wAwEAAAAh/hpDcmVhdGVkIHdpdGggYWpheGxvYWQuaW5mbwAh+QQJCgAAACwAAAAAEAAQAAADMwi63P4wyklrE2MIOggZnAdOmGYJRbExwroUmcG2LmDEwnHQLVsYOd2mBzkYDAdKa+dIAAAh+QQJCgAAACwAAAAAEAAQAAADNAi63P5OjCEgG4QMu7DmikRxQlFUYDEZIGBMRVsaqHwctXXf7WEYB4Ag1xjihkMZsiUkKhIAIfkECQoAAAAsAAAAABAAEAAAAzYIujIjK8pByJDMlFYvBoVjHA70GU7xSUJhmKtwHPAKzLO9HMaoKwJZ7Rf8AYPDDzKpZBqfvwQAIfkECQoAAAAsAAAAABAAEAAAAzMIumIlK8oyhpHsnFZfhYumCYUhDAQxRIdhHBGqRoKw0R8DYlJd8z0fMDgsGo/IpHI5TAAAIfkECQoAAAAsAAAAABAAEAAAAzIIunInK0rnZBTwGPNMgQwmdsNgXGJUlIWEuR5oWUIpz8pAEAMe6TwfwyYsGo/IpFKSAAAh+QQJCgAAACwAAAAAEAAQAAADMwi6IMKQORfjdOe82p4wGccc4CEuQradylesojEMBgsUc2G7sDX3lQGBMLAJibufbSlKAAAh+QQJCgAAACwAAAAAEAAQAAADMgi63P7wCRHZnFVdmgHu2nFwlWCI3WGc3TSWhUFGxTAUkGCbtgENBMJAEJsxgMLWzpEAACH5BAkKAAAALAAAAAAQABAAAAMyCLrc/jDKSatlQtScKdceCAjDII7HcQ4EMTCpyrCuUBjCYRgHVtqlAiB1YhiCnlsRkAAAOwAAAAAAAAAAAA==" />
            </div>
        </form>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
    data () {
        return {
            user: {
                name: '',
                email: '',
                tel: '',
                age: '',
                school: '',
                class: '',
                teamName: '',
                teamMembers: '',
                teamDescription: '',
                statementFile: '',
                privacyAccept: ''
            },
            submitted: false
        }
    },
    computed: {
        ...mapState('account', ['status'])
    },
    methods: {
        ...mapActions('account', ['register']),
        handleSubmit(e) {
            this.submitted = true;
            this.$validator.validate().then(valid => {
                if (valid) {
                    this.register(this.user);
                }
            });
        }
    }
};
</script>
