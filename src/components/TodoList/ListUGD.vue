<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

        <v-card>
            <v-card-title>
                <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details
                ></v-text-field>
                <v-spacer></v-spacer>
                <v-btn class="mr-3" color="blue" dark @click="dialogSelesai=true">
					Todo Selesai
				</v-btn>
                <v-btn color="success" dark @click="dialog = true">
                    Tambah
                </v-btn>
            </v-card-title>
            <v-data-table :headers="headers" :items="todos" :search="search">
                <template v-slot:[`item.priority`]="{ item }">
					<v-chip outlined label :color="getColor(item.priority)" dark>
						{{ item.priority }}
					</v-chip>
				</template>
                <template v-slot:[`item.actions`]="{ item }">
                    <v-btn small class="mr-2" @click="editItem(item)">
                        edit
                    </v-btn>
                    <v-btn small @click="deleteItem(item)">
                        delete
                    </v-btn>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>

            <v-card-title>
                <span class="headline">Form Todo</span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-text-field
                        v-model="formTodo.task"
                        label="Task"
                        required
                    ></v-text-field>

                    <v-select
                        v-model="formTodo.priority"
                        :items="['Penting', 'Biasa', 'Tidak penting']"
                        label="Priority"
                        required
                    ></v-select>

                    <v-textarea
                        v-model="formTodo.note"
                        label="Note"
                        required
                    ></v-textarea>
                </v-container>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="cancel">
                    Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save">
                    Save
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <v-dialog v-model="dialogEdit" persistent max-width="600px">
        <v-card>
            <v-card-title>
                <span class="headline">Form Todo - Edit</span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-text-field
                    v-model="formTodo.task"
                    label="Task"
                    required>                        
                    </v-text-field>

                    <v-select
                    v-model="formTodo.priority"
                    :items="['Penting','Biasa','Tidak penting']"
                    label="Priority"
                    required>
                    </v-select>

                    <v-textarea 
                    v-model="formTodo.note"
                    label="Note"
                    required>                     
                    </v-textarea>     
                </v-container>                        
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="cancel">
                    Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="update">
                    Update
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <v-dialog v-model="dialogDelete" persistent max-width="400px">
            <v-card>
                <v-card-title>
                    <span class="headline">Yakin ingin menghapus?</span>
                </v-card-title>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="green darken-1" text @click="cancel">Tidak</v-btn>
                    <v-btn color="red darken-1" text @click="confirmdelete">Ya</v-btn>
                </v-card-actions>

            </v-card>
    </v-dialog>
    <v-dialog v-model="dialogSelesai" persistent max-width="600px">
            <v-card>

            <v-card-title>
                <span class="headline">ToDo Selesai</span>
            </v-card-title>
                <v-card-text>
                <v-data-table>
                    <template v-slot:[`item.priority`]="{ item }">
                        <td>
                            <v-card v-if="item.priority == 'Penting'" style="border-color: red; color: red; width: fit-content; padding: 4px;" outlined>
                                {{ item.priority }}
                            </v-card>
                            <v-card v-else-if="item.priority == 'Biasa'" style="border-color: blue; color: blue; width: fit-content; padding: 4px;" outlined>
                                {{ item.priority }}
                            </v-card>
                            <v-card v-else outlined style="border-color: green; color: green; width: fit-content; padding: 4px;">
                                {{ item.priority }}
                            </v-card>
                        </td>
                    </template>
                    <template v-slot:[`item.actions`]="{ item }">
                        <v-btn small class="mr-2" @click="editItem(item)">
                            edit
                        </v-btn>
                        <v-btn small @click="deleteItem(item)">
                            delete
                        </v-btn>
                    </template>
                </v-data-table>
            </v-card-text>
            <v-btn color="blue darken-1" text @click="cancel">
                    Cancel
            </v-btn>
        </v-card>
    </v-dialog>

</v-main>
</template>
<script>
export default {
    name: "List",
    data() {
        return {
            search: null,
            dialog: false,
            dialogDelete: false,
            dialogEdit: false,
            dialogSelesai : false,
            headers: [
                {
                    text: "Task",
                    align: "start",
                    sortable: true,
                    value: "task",
                },
                
                { text: "Priority", value: "priority" },
                { text: "Note", value: "note" },
                { text: "Actions", value:"actions" },
            ],
            todos: [
                {
                    task: "bernafas",
                    priority: "Penting",
                    note: "huffttt",
                },
                {
                    task: "nongkrong",
                    priority: "Tidak penting",
                    note: "bersama tman2",
                },
                {
                    task: "masak",
                    priority: "Biasa",
                    note: "masak air 500ml",
                },
            ],
            selesai: [

            ],
            formTodo: {
                task: null,
                priority: null,
                note: null,
            },
        };
    },
    methods: {
        save() {
            this.todos.push(this.formTodo);
            this.resetForm();
            this.dialog = false;
        },
        cancel() {
            this.resetForm();
            this.dialog = false;
            this.dialogDelete = false;
            this.dialogSelesai = false;
        },
        resetForm() {
            this.formTodo = {
                task: null,
                priority: null,
                note: null,
            };
        },
        editItem(item){
            this.formTodo.task = item.task;
            this.formTodo.priority = item.priority;
            this.formTodo.note = item.note;
            this.dialogEdit = true;
            this.simpan = item;
        },
        update(){
            this.simpan.task=this.formTodo.task;
            this.simpan.priority= this.formTodo.priority;
            this.simpan.note = this.formTodo.note;
            this.dialogEdit = false;
        },
        deleteItem(item) {
            this.dialogDelete = true;
            this.edititem = item;
        },
        confirmdelete() {
            this.todos.splice(this.todos.indexOf(this.edititem), 1);
            this.dialogDelete = false;
        },
		getColor(prioritas) {
			if (prioritas === "Penting") return "red";
			else if (prioritas === "Biasa") return "blue";
			else return "green";
        },
     
    },
};
</script>