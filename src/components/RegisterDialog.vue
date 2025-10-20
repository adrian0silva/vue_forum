<template>
  <Form v-slot="{ handleSubmit }" :validation-schema="formSchema">
    <Dialog v-model:open="open">
      <DialogTrigger as-child>
        <Button variant="outline">Registrar</Button>
      </DialogTrigger>

      <DialogContent class="border-2 border-orange-200 bg-gradient-to-br from-orange-50 to-red-50 sm:max-w-md">
        <DialogHeader>
          <DialogTitle class="bg-gradient-to-r from-orange-600 to-red-600 bg-clip-text text-2xl font-bold text-transparent">
            Registrar-se
          </DialogTitle>
          <DialogDescription class="text-orange-600">
            Preencha seus dados Erisianos para começar sua jornada no caos.
          </DialogDescription>
        </DialogHeader>

        <!-- formulário -->
        <form id="dialogRegisterForm" @submit.prevent="handleSubmit(onSubmit)" class="space-y-4">
          <div class="space-y-2">
            <FormField v-slot="{ componentField }" name="email">
              <FormItem>
                <FormLabel for="email">E-mail</FormLabel>
                <FormControl>
                  <Input id="email" type="email" placeholder="seu@exemplo.com" v-bind="componentField" />
                </FormControl>
                <FormMessage />
              </FormItem>
            </FormField>
          </div>

          <div class="space-y-2">
            <FormField v-slot="{ componentField }" name="username">
              <FormItem>
                <FormLabel for="username">Username</FormLabel>
                <FormControl>
                  <Input id="username" type="text" placeholder="adriano_games" v-bind="componentField" />
                </FormControl>
                <FormMessage />
              </FormItem>
            </FormField>
          </div>

          <div class="space-y-2">
            <FormField v-slot="{ componentField }" name="password">
              <FormItem>
                <FormLabel for="password">Senha</FormLabel>
                <FormControl>
                  <Input id="password" type="password" placeholder="••••••••" v-bind="componentField" />
                </FormControl>
                <FormDescription>Deve ter no mínimo 6 caracteres.</FormDescription>
                <FormMessage />
              </FormItem>
            </FormField>

            <FormField v-slot="{ componentField }" name="confirmPassword">
              <FormItem>
                <FormLabel for="confirmPassword">Confirmar senha</FormLabel>
                <FormControl>
                  <Input id="confirmPassword" type="password" placeholder="••••••••" v-bind="componentField" />
                </FormControl>
                <FormDescription>Deve ser igual à senha.</FormDescription>
                <FormMessage />
              </FormItem>
            </FormField>

            <div class="col-span-4 flex flex-col gap-2 pt-2">
              <Button type="submit" class="w-full">
                Cadastrar
              </Button>

              <div class="flex items-center gap-2">
                <div class="h-px flex-1 bg-slate-200"></div>
                <span class="text-sm text-slate-500">ou</span>
                <div class="h-px flex-1 bg-slate-200"></div>
              </div>

              <Button variant="outline" class="w-full flex items-center justify-center gap-2" @click="onGoogleSignIn" type="button">
                <!-- SVG reduzido -->
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" viewBox="0 0 48 48">
                  <path fill="#fbc02d" d="M43.61 20.085h-..."></path>
                </svg>
                Entrar com Google
              </Button>
            </div>
          </div>
        </form>
      </DialogContent>
    </Dialog>
  </Form>
</template>

<script setup lang="ts">
import { toTypedSchema } from "@vee-validate/zod";
import * as z from "zod";
import { ref } from "vue";

import { Button } from "@/components/ui/button";
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { useToast } from "@/components/ui/toast/use-toast";
import axios from "axios";

const { toast } = useToast();
const open = ref(false);

/* Schema: email, username, password, confirmPassword (confirmação de senha) */
const formSchema = toTypedSchema(
  z
    .object({
      email: z.string().email({ message: "E-mail inválido" }),
      username: z.string().min(3, { message: "Username deve ter pelo menos 3 caracteres" }),
      password: z.string().min(6, { message: "Senha deve ter pelo menos 6 caracteres" }),
      confirmPassword: z.string().min(6, { message: "Confirmação deve ter pelo menos 6 caracteres" }),
    })
    .refine((data) => data.password === data.confirmPassword, {
      message: "As senhas não conferem",
      path: ["confirmPassword"],
    })
);

async function onSubmit(values: any) {
  try {
    // Ajuste a URL conforme seu backend (registro vs login)
    const res = await axios.post("http://localhost:3001/users", {
      email: values.email,
      login: values.username,
      password: values.password,
    });
    console.log("Resposta do servidor:", res.data);
    toast({ title: "Cadastro realizado", description: "Bem-vindo(a)!" });
    open.value = false;
  } catch (error: any) {
    console.error(error);
    toast({
      title: error?.response?.data?.message || "Erro ao fazer cadastro",
      description: "Verifique os dados e tente novamente.",
    });
  }
}

// Placeholder para login com Google — implemente o fluxo real (OAuth)
async function onGoogleSignIn() {
  try {
    toast({ title: "Entrando com Google...", description: "Abrindo fluxo de autenticação." });
    await new Promise((r) => setTimeout(r, 900));
    toast({ title: "Logado com Google", description: "Bem-vindo(a)!" });
    open.value = false;
  } catch (err) {
    toast({ title: "Erro", description: "Não foi possível autenticar com Google." });
  }
}
</script>

<style scoped>
/* Ajuste o svg do ícone do Google conforme quiser; coloquei placeholder para você trocar */
</style>
