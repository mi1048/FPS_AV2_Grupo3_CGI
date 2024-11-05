# FPS_AV2_Grupo3_CGI

Equipe da atv:
- Eric Menezes
- Matheus Sá
- João Borges
  
Atividades de Fundamentos de Programação de sistemas com o objetivo de configurar um servidor apache web e executar scripts CGI com Bash.

# Configuração de CGI no Ubuntu com Apache

Este guia explica como configurar o **Common Gateway Interface (CGI)** no Ubuntu, usando o servidor web **Apache**. O CGI permite a execução de scripts no servidor para gerar conteúdo dinâmico em resposta a solicitações de clientes.

## Pré-requisitos

- Ubuntu com privilégios de superusuário (root ou sudo).
- Apache instalado.

## Passo a Passo para Configuração

### 1. Instalar o Apache

Se o Apache ainda não estiver instalado, execute os comandos abaixo:

```bash
sudo apt update
sudo apt install apache2
```

### 2. Habilitar o Módulo CGI no Apache

Para que o Apache suporte scripts CGI, é necessário habilitar o módulo `mod_cgi`:

```bash
sudo a2enmod cgi
```

Em seguida, reinicie o Apache para aplicar as alterações:

```bash
sudo systemctl restart apache2
```

### 3. Configurar o Diretório para Scripts CGI

Por padrão, o Apache usa o diretório `/usr/lib/cgi-bin/` para scripts CGI. Edite o arquivo de configuração do Apache para garantir que este diretório esteja configurado corretamente:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

No arquivo, adicione a linha abaixo dentro do bloco `<VirtualHost>`:

```apache
ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
```

### 4. Permitir Execução de CGI no Diretório

Ainda no arquivo de configuração, defina as permissões para que o Apache possa executar scripts CGI dentro do diretório:

```apache
<Directory "/usr/lib/cgi-bin">
    Options +ExecCGI
    AddHandler cgi-script .cgi .pl .sh
    Require all granted
</Directory>
```

### 5. Criar um Script CGI de Teste

Para verificar se o CGI está funcionando, crie um script de teste no diretório `/usr/lib/cgi-bin/`. Neste exemplo, criaremos um script em Shell:

```bash
sudo nano /usr/lib/cgi-bin/test.sh
```

Adicione o seguinte conteúdo ao arquivo:

```bash
#!/bin/bash
echo "Content-type: text/html"
echo ""
echo "<html><body><h1>CGI Test</h1></body></html>"
```

### 6. Tornar o Script Executável

Dê permissão de execução ao script criado:

```bash
sudo chmod +x /usr/lib/cgi-bin/test.sh
```

### 7. Reiniciar o Apache

Para aplicar todas as configurações, reinicie o Apache:

```bash
sudo systemctl restart apache2
```

### 8. Testar o Script CGI

Abra o navegador e acesse o seguinte endereço para testar o script:

```
http://localhost/cgi-bin/test.sh
```

Você deve ver uma página com o título **CGI Test**.

### 9. Verificar Logs de Erro (Opcional)

Se o script não funcionar como esperado, consulte o log de erros do Apache para mais informações:

```bash
sudo tail -f /var/log/apache2/error.log
```

## Conclusão

Com estas etapas, você configurou o ambiente CGI no Ubuntu com Apache. Agora é possível executar scripts no servidor e gerar conteúdo dinâmico em resposta a interações do cliente. Para adicionar mais funcionalidades, crie scripts em linguagens como Shell, Perl ou Python e armazene-os no diretório CGI configurado.
