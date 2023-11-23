Documentação do App de Autenticação

# Login Activity (tela inicial/tela de login):

**Esse código é uma implementação básica de uma atividade (ou "Activity"
no contexto do Android) em um aplicativo Android que lida com a
autenticação de usuários usando o Firebase Authentication. Vou descrever
as principais partes do código para ajudar na compreensão:**

**Pacote e Importações:**

package com.example.appautenticao;

import androidx.appcompat.app.AppCompatActivity;

import android.app.Activity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.widget.Toast;

import com.example.appautenticao.databinding.ActivityLoginBinding;

import com.google.firebase.auth.FirebaseAuth;

**Aqui, o pacote do código é definido como "com.example.appautenticao",
e algumas classes e interfaces necessárias são importadas, incluindo as
relacionadas ao Android e ao Firebase Authentication.**

**Declaração da Classe:**

public class LoginActivity extends AppCompatActivity {

A classe LoginActivity estende a classe AppCompatActivity, que é uma
classe base para atividades que usam a biblioteca de suporte do Android.

**Atributos da Classe:**

private ActivityLoginBinding binding;

private FirebaseAuth mAuth;

**Aqui, estão sendo declaradas instâncias de ActivityLoginBinding e
FirebaseAuth como atributos da classe.**

**Método onCreate:**

@Override

protected void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

binding = ActivityLoginBinding.inflate(getLayoutInflater());

setContentView(binding.getRoot());

mAuth = FirebaseAuth.getInstance();

// Configuração de cliques nos elementos da interface

binding.textCadastro.setOnClickListener(v -> {

startActivity(new Intent(this, CadastroActivity.class));

});

binding.btnLogin.setOnClickListener(v -> validaDados());

binding.textRecuperaConta.setOnClickListener(v -> startActivity(new
Intent(this, RecuperaContaActivity.class)));

}

**O método onCreate é chamado quando a atividade é criada. Ele configura
a interface do usuário definida no arquivo de layout
ActivityLoginBinding e configura os cliques nos botões e textos da
interface.**

**Método validaDados:**

private void validaDados() {

// Obtém e valida o email e a senha

String email = binding.editEmail.getText().toString().trim();

String senha = binding.editSenha.getText().toString().trim();

if (!email.isEmpty()) {

if (!senha.isEmpty()) {

binding.progressBar.setVisibility(View.VISIBLE);

loginFirebase(email, senha);

} else {

Toast.makeText(this, "Informe uma senha.", Toast.LENGTH_SHORT).show();

}

} else {

Toast.makeText(this, "Informe seu e-mail.", Toast.LENGTH_SHORT).show();

}

}

**Este método é responsável por validar os dados de entrada do usuário
(email e senha) e chamar o método loginFirebase se os dados forem
válidos.**

**Método loginFirebase:**

private void loginFirebase(String email, String senha) {

// Autentica o usuário no Firebase

mAuth.signInWithEmailAndPassword(email,
senha).addOnCompleteListener(task -> {

if (task.isSuccessful()) {

// Se a autenticação for bem-sucedida, inicia a MainActivity

finish();

startActivity(new Intent(this, MainActivity.class));

} else {

// Se houver um erro na autenticação, exibe uma mensagem de erro

binding.progressBar.setVisibility(View.GONE);

Toast.makeText(this, "Ocorreu um erro", Toast.LENGTH_SHORT).show();

}

});

}

**Este método utiliza o Firebase Authentication para tentar autenticar o
usuário com o email e senha fornecidos. Se a autenticação for
bem-sucedida, a atividade é encerrada e a MainActivity é iniciada. Se
houver um erro na autenticação, uma mensagem de erro é exibida.**

# Cadastro Activity (tela de cadastro):

**Este código implementa uma atividade em um aplicativo Android chamada
CadastroActivity. Essa atividade está relacionada ao processo de criação
de uma nova conta de usuário, utilizando o Firebase Authentication. Vou
descrever as partes principais do código:**

**Pacote e Importações:**

package com.example.appautenticao;

import androidx.annotation.NonNull;

import androidx.appcompat.app.AppCompatActivity;

import android.app.Activity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

import android.widget.EditText;

import android.widget.ProgressBar;

import android.widget.Toast;

import com.example.appautenticao.databinding.ActivityCadastroBinding;

import com.google.android.gms.tasks.OnCompleteListener;

import com.google.android.gms.tasks.Task;

import com.google.firebase.auth.AuthResult;

import com.google.firebase.auth.FirebaseAuth;

As importações incluem classes relacionadas ao Android, Firebase
Authentication e outras dependências necessárias.

**Declaração da Classe:**

java

Copy code

public class CadastroActivity extends AppCompatActivity {

A classe CadastroActivity estende a classe AppCompatActivity, indicando
que é uma atividade Android.

**Atributos da Classe:**

java

Copy code

private FirebaseAuth mAuth;

private EditText editEmail;

private EditText editSenha;

private Button btnCriarConta;

private ProgressBar progressBar;

private ActivityCadastroBinding binding;

**Esses são os atributos da classe, representando instâncias de
FirebaseAuth (para autenticação), EditText (para entrada de email e
senha), Button (para criar conta) e ProgressBar (para indicar o
progresso).**

**Método onCreate:**

@Override

protected void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

binding = ActivityCadastroBinding.inflate(getLayoutInflater());

setContentView(binding.getRoot());

mAuth = FirebaseAuth.getInstance();

binding.btnLogin.setOnClickListener(v -> validaDados());

}

**O método onCreate é chamado quando a atividade é criada. Ele configura
a interface do usuário definida no arquivo de layout
ActivityCadastroBinding e define um clique no botão de login para chamar
o método validaDados().**

**Método validaDados:**

private void validaDados() {

String email = binding.editEmail.getText().toString().trim();

String senha = binding.editSenha.getText().toString().trim();

if (!email.isEmpty()) {

if (!senha.isEmpty()) {

binding.progressBar.setVisibility(View.VISIBLE);

criarContaFirebase(email, senha);

} else {

Toast.makeText(this, "Informe uma senha.", Toast.LENGTH_SHORT).show();

}

} else {

Toast.makeText(this, "Informe seu e-mail.", Toast.LENGTH_SHORT).show();

}

}

**Este método é responsável por validar os dados de entrada do usuário
(email e senha) e chamar o método criarContaFirebase se os dados forem
válidos.**

**Método criarContaFirebase:**

private void criarContaFirebase(String email, String senha) {

mAuth.createUserWithEmailAndPassword(email,
senha).addOnCompleteListener(task -> {

if (task.isSuccessful()) {

// Se a criação de conta for bem-sucedida, inicia a MainActivity

startActivity(new Intent(this, MainActivity.class));

} else {

// Se houver um erro na criação de conta, exibe uma mensagem de erro

Toast.makeText(this, "Ocorreu um erro", Toast.LENGTH_SHORT).show();

}

});

}

**Este método utiliza o Firebase Authentication para criar uma nova
conta de usuário com o email e senha fornecidos. Se a criação de conta
for bem-sucedida, a atividade é encerrada e a MainActivity é iniciada.
Se houver um erro na criação de conta, uma mensagem de erro é exibida.**

**Este código faz parte de um aplicativo Android que gerencia o processo
de criação de contas de usuários utilizando o Firebase Authentication.
Ele inclui manipulação de interface do usuário, entrada de dados,
validação e interação com o Firebase para criar novas contas de
usuário.**

# RecuperaConta Activity (recuperação de senha):

**Esse código implementa uma atividade (RecuperaContaActivity) em um
aplicativo Android, que é responsável por permitir que os usuários
solicitem a recuperação de suas contas por meio do envio de e-mails de
redefinição de senha. Abaixo estão as principais características do
código:**

**Pacote e Importações:**

package com.example.appautenticao;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;

import android.os.Bundle;

import android.view.View;

import android.widget.Toast;

import
com.example.appautenticao.databinding.ActivityRecuperaContaBinding;

import com.google.firebase.auth.FirebaseAuth;

**As importações incluem classes relacionadas ao Android, Firebase
Authentication e outras dependências necessárias.**

**Declaração da Classe:**

public class RecuperaContaActivity extends AppCompatActivity {

**A classe RecuperaContaActivity estende a classe AppCompatActivity,
indicando que é uma atividade Android.**

**Atributos da Classe**:

private FirebaseAuth mAuth;

private ActivityRecuperaContaBinding binding;

**Esses são os atributos da classe, representando instâncias de
FirebaseAuth (para autenticação) e ActivityRecuperaContaBinding (para
vincular com a interface definida no arquivo de layout).**

**Método onCreate:**

@Override

protected void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

binding = ActivityRecuperaContaBinding.inflate(getLayoutInflater());

setContentView(binding.getRoot());

mAuth = FirebaseAuth.getInstance();

binding.btnRecuperaConta.setOnClickListener(v -> validaDados());

}

**O método onCreate é chamado quando a atividade é criada. Ele configura
a interface do usuário definida no arquivo de layout
ActivityRecuperaContaBinding e define um clique no botão para chamar o
método validaDados().**

**Método validaDados:**

private void validaDados() {

String email = binding.editEmail.getText().toString().trim();

if (!email.isEmpty()) {

binding.progressBar.setVisibility(View.VISIBLE);

recuperaContaFirebase(email);

} else {

Toast.makeText(this, "Informe seu e-mail.", Toast.LENGTH_SHORT).show();

}

}

**Este método é responsável por validar os dados de entrada do usuário
(apenas o email neste caso) e chama o método recuperaContaFirebase se os
dados forem válidos.**

**Método recuperaContaFirebase:**

private void recuperaContaFirebase(String email) {

mAuth.sendPasswordResetEmail(email).addOnCompleteListener(task -> {

if (task.isSuccessful()) {

// Se o e-mail de redefinição for enviado com sucesso, encerra a
atividade

finish();

Toast.makeText(this, "Por favor, verifique o seu e-mail",
Toast.LENGTH_SHORT).show();

} else {

// Se houver um erro no envio do e-mail, exibe uma mensagem de erro

binding.progressBar.setVisibility(View.GONE);

Toast.makeText(this, "Ocorreu um erro", Toast.LENGTH_SHORT).show();

}

});

}

**Este método utiliza o Firebase Authentication para enviar um e-mail de
redefinição de senha para o endereço de e-mail fornecido pelo usuário.
Se o e-mail for enviado com sucesso, a atividade é encerrada, e uma
mensagem de sucesso é exibida. Se houver um erro no envio do e-mail, uma
mensagem de erro é exibida.**

**Em resumo, este código faz parte de um aplicativo Android e permite
que os usuários solicitem a recuperação de suas contas por meio do envio
de e-mails de redefinição de senha usando o Firebase Authentication. Ele
inclui manipulação de interface do usuário, entrada de dados, validação
e interação com o Firebase para recuperar contas de usuário.**

# Main Activity (menu principal):

**Este código representa a implementação de uma atividade principal
(MainActivity) em um aplicativo Android. A atividade possui um botão e
um texto na interface do usuário, e ao clicar no botão, uma tarefa
assíncrona (Tarefa) é iniciada para realizar uma consulta HTTP a uma API
pública de CEP (Código de Endereçamento Postal) no Brasil.**

**Aqui estão os principais pontos do código:**

**Pacote e Importações:**

package com.example.appautenticao;

import androidx.appcompat.app.AppCompatActivity;

import android.os.AsyncTask;

import android.os.Bundle;

import android.view.View;

import android.widget.Button;

import android.widget.TextView;

import com.example.appautenticao.databinding.ActivityMainBinding;

import
com.example.appautenticao.databinding.ActivityRecuperaContaBinding;

import java.io.BufferedReader;

import java.io.IOException;

import java.io.InputStreamReader;

import java.net.HttpURLConnection;

import java.net.URL;

**As importações incluem classes relacionadas ao Android, recursos de
interface do usuário e classes necessárias para realizar uma consulta
HTTP.**

**Declaração da Classe:**

public class MainActivity extends AppCompatActivity {

A classe MainActivity estende a classe AppCompatActivity, **indicando
que é uma atividade Android.**

**Atributos da Classe:**

private Button button;

private TextView textView;

private ActivityMainBinding binding;

Esses são os atributos da classe, representando instâncias de Button e
TextView e ActivityMainBinding para vincular a interface definida no
arquivo de layout.

**Método onCreate:**

@Override

protected void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

binding = ActivityMainBinding.inflate(getLayoutInflater());

setContentView(binding.getRoot());

button = button.findViewById(R.id.btnConsultar);

textView = textView.findViewById(R.id.imageView);

button.setOnClickListener(new View.OnClickListener() {

@Override

public void onClick(View v) {

Tarefa tarefa = new Tarefa();

tarefa.execute("https://viacep.com.br/ws/01001000/json/");

}

});

}

**O método onCreate é chamado quando a atividade é criada. Ele configura
a interface do usuário definida no arquivo de layout ActivityMainBinding
e define um clique no botão para iniciar a tarefa assíncrona (Tarefa)
quando o botão é clicado. A URL fornecida para a tarefa é um exemplo de
consulta à API ViaCEP para o CEP 01001000.**

**Classe Interna Tarefa (AsyncTask):**

private class Tarefa extends AsyncTask\<String, String, String>{

@Override

protected String doInBackground(String... strings){

String retorno = conexao.getDados(strings\[0\]);

return retorno;

}

@Override

protected void onPostExecute(String s){

textView.setText(s);

}

}

**Esta é uma classe interna (Tarefa) que estende AsyncTask. O método
doInBackground é responsável por executar a tarefa em segundo plano,
neste caso, fazendo uma chamada HTTP para a URL fornecida. O método
onPostExecute é chamado quando a tarefa é concluída, e neste caso, ele
atualiza o texto na textView com o resultado da consulta.**

**O código também faz referência a um objeto conexao que não está
explicitamente definido no código fornecido, possivelmente é uma
instância de uma classe de conexão HTTP.**

**Em resumo, este código representa uma atividade Android (MainActivity)
que realiza uma consulta HTTP assíncrona para obter informações de
endereço com base em um CEP usando a API ViaCEP. A resposta da API é
exibida em um componente de texto (textView) na interface do usuário
após o clique em um botão.**

# Classe Conexão (Conecta com a API):

**Este código implementa uma classe chamada conexao que fornece uma
funcionalidade de conexão HTTP para realizar solicitações GET. Essa
classe parece ser usada para obter dados de uma URL, e ela se encaixa no
contexto dos códigos anteriores que realizam consultas a uma API de CEP
brasileira (ViaCEP).**

**Aqui estão os pontos principais do código:**

**Pacote e Importações:**

package com.example.appautenticao;

**Classe conexao:**

public class conexao {

**A classe conexao é declarada como pública, e ela possui um método
estático chamado getDados que recebe uma URI como parâmetro e retorna os
dados obtidos da solicitação HTTP.**

**Método getDados:**

public static String getDados(String uri){

BufferedReader bufferedReader = null;

try{

URL url = new URL(uri);

HttpURLConnection httpURLConnection = (HttpURLConnection)
url.openConnection();

StringBuilder stringBuilder = new StringBuilder();

bufferedReader = new BufferedReader(new
InputStreamReader(httpURLConnection.getInputStream()));

String linha;

while ((linha=bufferedReader.readLine()) != null){

stringBuilder.append(linha+"/n");

}

return stringBuilder.toString();

} catch (Exception e) {

e.printStackTrace();

return null;

} finally {

if (bufferedReader != null) {

try {

bufferedReader.close();

} catch (IOException e) {

e.printStackTrace();

}

}

}

}

**O método getDados realiza uma solicitação HTTP GET para a URI
fornecida.**

**Ele utiliza uma HttpURLConnection para abrir a conexão.**

**Um BufferedReader é utilizado para ler os dados da resposta.**

**Os dados lidos são armazenados em um StringBuilder.**

**O método retorna os dados em formato de string.**

**Tratamento de Exceções:**

**O código inclui tratamento de exceções para lidar com erros durante a
execução da solicitação HTTP.**

**Se ocorrer uma exceção, a pilha de exceções é impressa, e o método
retorna null.**

**A cláusula finally garante que o BufferedReader seja fechado,
independentemente de qualquer exceção.**

**Uso da Classe conexao nos Códigos Anteriores:**

**Nos códigos anteriores, especialmente nas atividades CadastroActivity
e MainActivity, uma instância de conexao (ou um método estático dela) é
usada para realizar solicitações HTTP, como visto em Tarefa da
MainActivity.**

**Em resumo, este código (conexao) é uma classe utilitária que fornece
funcionalidades básicas para realizar solicitações HTTP. Ela é utilizada
nos códigos anteriores para obter dados da API ViaCEP.**

# Instruções de instalação das tecnologias escolhidas:

**Firebase Authentication:**

**Adicione a dependência no arquivo build.gradle (app-level) para o
Firebase Authentication:**

**implementation 'com.google.firebase:firebase-auth:22.0.0'**

**Certifique-se de ter o plugin do Firebase configurado no arquivo
build.gradle (project-level).**

**Binding no Android:**

**Os códigos fazem uso do View Binding e Data Binding para facilitar a
manipulação de elementos da interface do usuário. Para habilitar o View
Binding, adicione no arquivo build.gradle (app-level):**

android {

...

viewBinding {

enabled = true

}

}

**No caso do Data Binding, adicione:**

android {

...

buildFeatures {

dataBinding true

}

}

**Passo a passo do desenvolvimento do projeto:**

**Configuração do Firebase Authentication:**

**Configure o Firebase no console
(https://console.firebase.google.com/).**

**Adicione o aplicativo ao projeto no Firebase.**

**Adquira o arquivo de configuração google-services.json e coloque-o no
diretório do seu aplicativo.**

**Desenvolvimento da Tela de Login (LoginActivity):**

**Utilização do layout definido no arquivo ActivityLoginBinding usando
View Binding.**

**Configuração de cliques nos elementos da interface (botão de login,
texto de cadastro, texto de recuperação de conta).**

**Validação dos dados de entrada (email e senha).**

**Autenticação do usuário usando Firebase Authentication.**

**Desenvolvimento da Tela de Cadastro (CadastroActivity):**

**Utilização do layout definido no arquivo ActivityCadastroBinding
usando View Binding.**

**Configuração de cliques nos elementos da interface (botão de criar
conta, texto de login).**

**Validação dos dados de entrada (email e senha).**

**Criação de uma nova conta de usuário usando Firebase Authentication.**

**Desenvolvimento da Tela de Recuperação de Conta
(RecuperaContaActivity):**

**Utilização do layout definido no arquivo ActivityRecuperaContaBinding
usando View Binding.**

**Configuração de clique no botão para iniciar o processo de recuperação
de conta.**

**Validação do email fornecido.**

**Envio de e-mail de redefinição de senha usando Firebase
Authentication.**

**Desenvolvimento da Tela Principal (MainActivity):**

**Utilização do layout definido no arquivo ActivityMainBinding usando
View Binding.**

**Configuração de clique no botão para iniciar a consulta à API
ViaCEP.**

**Implementação de uma classe de conexão (conexao) para realizar
solicitações HTTP.**

**Utilização de uma tarefa assíncrona (AsyncTask) para executar a
consulta em segundo plano.**

**Exibição dos resultados da consulta na textView.**

# Trechos de Código Relevantes:

**Firebase Authentication:**

**Criação de uma nova conta:**

mAuth.createUserWithEmailAndPassword(email,
senha).addOnCompleteListener(task -> {

if (task.isSuccessful()) {

startActivity(new Intent(this, MainActivity.class));

} else {

Toast.makeText(this, "Ocorreu um erro", Toast.LENGTH_SHORT).show();

}

});

**Login de um usuário existente:**

mAuth.signInWithEmailAndPassword(email,
senha).addOnCompleteListener(task -> {

if (task.isSuccessful()) {

finish();

startActivity(new Intent(this, MainActivity.class));

} else {

binding.progressBar.setVisibility(View.GONE);

Toast.makeText(this, "Ocorreu um erro", Toast.LENGTH_SHORT).show();

}

});

**Recuperação de conta por e-mail:**

mAuth.sendPasswordResetEmail(email).addOnCompleteListener(task -> {

if (task.isSuccessful()) {

finish();

Toast.makeText(this, "Por favor, verifique o seu e-mail",
Toast.LENGTH_SHORT).show();

} else {

binding.progressBar.setVisibility(View.GONE);

Toast.makeText(this, "Ocorreu um erro", Toast.LENGTH_SHORT).show();

}

});

**Consulta à API ViaCEP:**

**Implementação da classe conexao para realizar solicitações HTTP:**

public static String getDados(String uri){

BufferedReader bufferedReader = null;

try{

URL url = new URL(uri);

HttpURLConnection httpURLConnection = (HttpURLConnection)
url.openConnection();

StringBuilder stringBuilder = new StringBuilder();

bufferedReader = new BufferedReader(new
InputStreamReader(httpURLConnection.getInputStream()));

String linha;

while ((linha=bufferedReader.readLine()) != null){

stringBuilder.append(linha+"/n");

}

return stringBuilder.toString();

} catch (Exception e) {

e.printStackTrace();

return null;

}finally {

if (bufferedReader != null) {

try {

bufferedReader.close();

} catch (IOException e) {

e.printStackTrace();

}

}

}

}

**Uso da classe conexao para realizar a consulta:**

Tarefa tarefa = new Tarefa();

tarefa.execute("https://viacep.com.br/ws/01001000/json/");

Classe AsyncTask para execução assíncrona da consulta:

private class Tarefa extends AsyncTask\<String, String, String>{

@Override

protected String doInBackground(String... strings){

String retorno = conexao.getDados(strings\[0\]);

return retorno;

}

@Override

protected void onPostExecute(String s){

textView.setText(s);

}

}

**Esses trechos de código representam a implementação de funcionalidades
chave, incluindo autenticação Firebase, validação de dados, interação
com a API ViaCEP, e utilização de tarefas assíncronas para garantir uma
experiência de usuário responsiva.**

# Tutorial de Utilização do Aplicativo

## Tela de Login:

**Na tela inicial, clique no botão de "Cadastro" para criar uma nova
conta.**

<img [Print1](https://github.com/JecaDev/AppAutentica-o/assets/129800662/20d1ebce-8447-4688-9b7c-eb3ea3a14f10)/>

### 

### 

### 

### Preenchimento de Dados:

**Insira um endereço de e-mail e uma senha nos campos correspondentes.**

**Clique no botão de criação de conta.**

### <img ![Print1](https://github.com/JecaDev/AppAutentica-o/assets/129800662/183ed915-6751-46c0-8f63-aaf99fdcbc20)/>


### Confirmação:

**Se a criação da conta for bem-sucedida, você será redirecionado para a
tela principal do aplicativo.**

<img ![Print2](https://github.com/JecaDev/AppAutentica-o/assets/129800662/971adf63-ccf1-45da-820e-af7adbec2102)/>

## Login

### Tela Inicial:

**Se você já possui uma conta, insira seu e-mail e senha na tela inicial
de login.**

**Clique no botão de login.**

### Acesso à Tela Principal:

**Se as credenciais estiverem corretas, você será redirecionado para a
tela principal do aplicativo.**

**Recuperação de Senha**

## Esqueceu a Senha:

**Se você esqueceu sua senha, clique no link de "Recuperar Conta" na
tela de login.**

<img![Print3](https://github.com/JecaDev/AppAutentica-o/assets/129800662/f4e14096-ba74-4d56-b528-dedf487c6aca)/>

### Preenchimento de E-mail:

**Insira o endereço de e-mail associado à sua conta.**

**Clique no botão para enviar um e-mail de recuperação.**

### **Verificação de E-mail:**

**Verifique seu e-mail para obter as instruções de recuperação.**

**Siga as instruções para redefinir sua senha.**

## Consulta de CEP

## Tela Principal:

**Na tela principal, clique no botão designado para a consulta de CEP.**

### **Resultado da Consulta:**

**Após a consulta, os detalhes do CEP serão exibidos na tela.**

<img![Print1](https://github.com/JecaDev/AppAutentica-o/assets/129800662/4c061c7f-1a26-44d4-b672-43fad880775e)/>

## Observações:

### Segurança da Conta:

**Mantenha suas credenciais de conta seguras e não compartilhe com
terceiros.**

**Evite utilizar senhas simples e fácil de adivinhar.**

**Esse tutorial visa simplificar o processo de utilização do aplicativo.
Certifique-se de estar utilizando uma conexão de internet adequada para
garantir o funcionamento correto da consulta à API ViaCEP.**
