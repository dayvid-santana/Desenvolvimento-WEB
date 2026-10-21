### Como funciona o login integrado

O usuário informa suas credenciais na aplicação. O servidor as envia ao serviço corporativo de autenticação por uma conexão segura e aguarda a confirmação.

Se o acesso for aprovado, o servidor cria uma sessão própria para o usuário. A partir daí, a aplicação usa essa sessão para reconhecer o usuário nas próximas requisições. A senha não precisa ser guardada pela aplicação, e os dados temporários recebidos durante a validação não devem ser usados como sessão da aplicação.

Em resumo: **o serviço corporativo confirma a identidade; a aplicação cria e controla sua própria sessão.**

Esse fluxo automatiza o envio de credenciais para uma página de login. Ele é diferente de uma federação de identidade padronizada, na qual um provedor autentica o usuário e retorna uma confirmação por meio de um protocolo específico.


<img width="1672" height="941" alt="Imagem do ChatGPT 21 de out  de 2026, 11_06_20" src="https://github.com/user-attachments/assets/6e40816f-066f-4765-812f-151dad6e7fa2" />


### Login integrado, passo a passo

Há dois sistemas envolvidos: **a aplicação que o usuário quer acessar** e **o serviço que já conhece as credenciais dele**.

1. **O usuário inicia o login**  
   Na tela de acesso, ele informa suas credenciais. Elas são enviadas ao servidor da aplicação por uma conexão segura. O navegador não deve conversar diretamente com o serviço externo nem guardar tokens dele.

2. **O servidor prepara a validação**  
   O serviço externo pode exigir campos temporários gerados para aquela tentativa de login. O servidor obtém esses campos e prepara a requisição no formato esperado. Se necessário, protege a senha conforme o mecanismo oferecido pelo próprio serviço.

3. **O serviço externo verifica as credenciais**  
   O servidor envia a tentativa de autenticação e interpreta a resposta. Uma confirmação significa que o serviço reconheceu as credenciais. Uma rejeição significa que o acesso não foi confirmado; a aplicação deve apresentar uma mensagem clara sem revelar detalhes internos.

4. **A aplicação cria sua própria sessão**  
   Após a confirmação, o servidor da aplicação cria uma sessão local. Ela representa o usuário dentro da aplicação e é usada para autorizar as próximas requisições. A aplicação não deve usar a senha nem os tokens temporários do serviço externo como se fossem essa sessão.

5. **O navegador recebe apenas o necessário para continuar**  
   O servidor entrega um identificador de sessão protegido, normalmente em um cookie com atributos de segurança. Nas próximas operações, o navegador envia esse cookie e o servidor verifica se a sessão continua válida.

6. **A sessão pode terminar ou ser revogada**  
   Ao sair, a sessão deve ser encerrada. Ela também pode expirar ou ser revogada pelo servidor. Depois disso, o usuário precisa autenticar-se novamente.

### Quem faz o quê?

- **O serviço externo** confirma se as credenciais são válidas.
- **O servidor da aplicação** coordena a validação, identifica o usuário e cria a sessão local.
- **O navegador** coleta as credenciais e mantém o cookie de sessão; não deve receber a senha de volta nem os tokens temporários do serviço externo.

### Por que existem duas etapas?

A confirmação externa responde: **“Essas credenciais pertencem a um usuário reconhecido?”**  
A sessão local responde: **“Este navegador já foi autenticado nesta aplicação?”**

Separar as duas etapas permite que a aplicação controle suas sessões sem depender de guardar a senha do usuário ou reutilizar credenciais temporárias de outro sistema.

### Uma analogia

Pense em um prédio com recepção e salas internas. A recepção confere a identidade do visitante. Depois da confirmação, o prédio emite um crachá próprio, válido para circular dentro dele. A recepção confirma quem é a pessoa; o crachá representa a sessão dela naquele prédio.

### Observação sobre o tipo de integração

Quando o servidor automatiza o envio das credenciais a uma página de login, ele depende do formato e do comportamento dessa página. Isso é diferente de uma integração federada padronizada, na qual um provedor autentica o usuário e devolve uma confirmação por um protocolo próprio.
