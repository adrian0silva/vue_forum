<template>
  <Form v-slot="{ handleSubmit }" as="" keep-values :validation-schema="formSchema">
    <Dialog v-model:open="open">
      <DialogTrigger>
        <Button variant="outline">Login</Button>
      </DialogTrigger>

      <DialogContent class="sm:max-w-[425px]">
        <DialogHeader>
          <DialogTitle>Entrar</DialogTitle>
          <DialogDescription>
            Faça login com seu e-mail ou use o Google.
          </DialogDescription>
        </DialogHeader>

        <form id="dialogForm" @submit="handleSubmit($event, onSubmit)" class="space-y-4">
          <div className="space-y-2">
            
            <FormField v-slot="{ componentField }" name="email">
              <FormItem>
                <FormLabel for="email">E-mail</FormLabel>
                <FormControl>
                  <Input id="email" type="email" placeholder="seu@exemplo.com" v-bind="componentField" />
                </FormControl>

                <FormMessage  />
              </FormItem>
            </FormField>
          </div>
          <div className="space-y-2">
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

            <div class="col-span-4 flex flex-col gap-2 pt-2">
              <Button type="submit" form="dialogForm" class="w-full">
                Entrar
              </Button>

              <div class="flex items-center gap-2">
                <div class="h-px flex-1 bg-slate-200"></div>
                <span class="text-sm text-slate-500">ou</span>
                <div class="h-px flex-1 bg-slate-200"></div>
              </div>

              <Button variant="outline" class="w-full flex items-center justify-center gap-2" @click="onGoogleSignIn" type="button">
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

// Schema: e-mail e senha
const formSchema = toTypedSchema(
  z.object({
    email: z.string().email({ message: "E-mail inválido" }),
    password: z.string().min(6, { message: "Senha deve ter pelo menos 6 caracteres" }),
  })
);

const open = ref(false);

async function onSubmit(values: any) {
  await axios.post('http://localhost:3001/auth/login', {
    email: values.email,
    password: values.password
  })
  .then((res) => {
    console.log(res.data);
    open.value = false;
  })
  .catch((error) => {
    console.log(error);
    toast({
    title: error.response?.data.message || "Erro ao fazer login",
    description: "Verifique suas credenciais e tente novamente.",
  });
  })
}

// Placeholder para login com Google — implemente o fluxo real (OAuth)
async function onGoogleSignIn() {
  try {
    toast({ title: "Entrando com Google...", description: "Abrindo fluxo de autenticação." });

    // Simular comportamento de popup/oauth:
    await new Promise((r) => setTimeout(r, 900));

    // Simule sucesso
    toast({ title: "Logado com Google", description: "Bem-vindo(a)!" });
    open.value = false;
  } catch (err) {
    toast({ title: "Erro", description: "Não foi possível autenticar com Google." });
  }
}

//
</script>

<style scoped>
/* Ajuste o svg do ícone do Google conforme quiser; coloquei placeholder para você trocar */
</style>
