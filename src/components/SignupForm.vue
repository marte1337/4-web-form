<template>
    <!-- Form Submit: .prevent blockiert das neuladen der Seite, nach dem Submit -->
    <form @submit.prevent="handleSubmit">

        <label>Email:</label>
        <!-- Two Way Data Binding über v-model: Binding von Input -> Data Properties -> Template  -->
        <input type="email" required v-model="email">
        <label>Password:</label>  
        <input type="password" required v-model="password">
        <div v-if="passwordError" class="error">{{ passwordError }}</div>

        <label>Role:</label>
        <select v-model="role">
            <option value="developer">Web Developer</option>
            <option value="designer">Web Designer</option>
        </select>


        <!-- Eine Input-Box die beliebig viele individuelle Skills aufnimmt
        und jeweils nach einem Enter-KeyEvent in einem Array-Container speichert  -->
        <label>Input your Skills: <br>(press Spacebar after each skill / Click to delete)</label>
        <input type="text" v-model="tempSkill" @keyup="addSkill">
        <div v-for="skill in skills" :key="skill" class="pill">
            <span @click="deleteSkill(skill)">{{ skill }}</span>
        </div>


        <!-- Single Checkbox = Boolean -->
        <div class="terms">
            <input type="checkbox" required v-model="terms">
            <label>Accept terms and conditions</label>
        </div>

        <!-- Multiple Checkboxes = Array -->
        <!-- <div>
            <input type="checkbox" value="shaun" v-model="names">
            <label>Shaun</label>
        </div>
        <div>
            <input type="checkbox" value="yoshi" v-model="names">
            <label>Yoshi</label>
        </div>
        <div>
            <input type="checkbox" value="mario" v-model="names">
            <label>Mario</label>
        </div> -->

        <div class="submit">
            <button>Create an Account</button>
        </div>

    </form>
    <p>Email: {{ email }}</p>
    <p>Password: {{ password }}</p>
    <p>Role: {{ role }}</p>
    <p>Accept Terms: {{ terms }}</p>
    <!-- <p>Names: {{ names }}</p> -->
    <p>TempSkills: {{ tempSkill }}</p>
    <p>Skills: {{ skills }}</p>
</template>





<script>
export default {
    data() {
        return {
            //Datencontainer für v-model (frei Def)
            email: "",
            password: "",
            role: "developer",
            terms: false,
            // names: [],

            //tempSkill ist für den aktuell dargestellten Skill
            //skills ist der Ort, an dem gespeichert wird
            tempSkill: "",
            skills: [],

            passwordError: ""
        }
    },
    methods: {

        // - "e" (frei Def) = Event-Objekt wird autom in die Event-Funktion geliefert
        // - Event-Objekt enthält alle Infos zum Event, lässt sich auch loggen - e.key ist ist der Value für die gedrückte Taste
        // - "this.tempSkill" im Statement als Sicherung, dass ein Value vor dem Komma eingetippt wurde,
        //  innerhalb der Funktion wird der tempSkill-Value dann ins skills-Array gepusht, anschließend
        //  wird der tempSkill-Value wieder gelöscht
        // - Vor dem Push wird noch mit einem zweiten if-Statem. abgesichert, dass sich der aktuelle tempSkill nicht schon im Array befindet 
        //   Damit werden Dopplungen ausgeschlossen. Das ist auch wichtig wür den :key-Bind "skill" im v-for-Loop oben (der muss individuell sein)
        addSkill(e) {
            if (e.key === " " && this.tempSkill) {
                if (!this.skills.includes(this.tempSkill)) {
                    this.skills.push(this.tempSkill)
                }
                this.tempSkill = ""
            }
        },


        // - "skill" wird als Argument oben im click-Event manuell eingespeist (dort kann es aus dem v-for-Loop abgegriffen werden)
        //   Hätte man oben nichts eingegeben, wäre wieder standardmäßig das Event-Objekt geliefert worden 
        // - .filter iterated durch das Array und filtert wenn "true" entsprechende items aus dem Array. Wenn das oben eingespeißte 
        //   "skill"-Argument also = item ist, ist der return false und der skill wird gelöscht. 
        //   (Wenn skill ungleich item ist, returnt true und nichts passiert; dies ist in diesem Fall aber nicht möglich, da ja das item = das skill-Argument ist) 
        deleteSkill(skill) {
            this.skills = this.skills.filter((item) => {
                return skill !== item
            })
        },


        handleSubmit() {
            // validate PW
            this.passwordError = this.password.length > 5 ? "" : "Password must be at least 6 chars long"

            if (!this.passwordError) {
                console.log("email: ", this.email)
                console.log("password: ", this.password)
                console.log("role: ", this.role)
                console.log("skills: ", this.skills)
                console.log("terms accepted: ", this.terms)
            }
        }
    }

}
</script>


<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input, select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}
.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 14px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}
button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit {
    text-align: center;
}
.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>