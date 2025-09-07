# tcb_site_estatico_cloudfront_s3

Criação de um Site Estatico utilizando o Cloudfront e hospedagem no S3

Para testarmos a criação no AWS S3, foi criado um bucket com o nome "tcb-aws-bootcamp-noronha-01" minha conta de teste na AWS "030847345550" na Region: 'us-east-1'



Abaixo apresentamos um passo a passo de como implementar:


# Módulo 4 **| Task 1 - Acessando site hospedado em um Bucket S3 através do Amazon CloudFront**

### Atenção!!!

### Essa documentação é constantemente revisada e atualizada conforme mudanças que ocorrem a todo momento nos provedores de cloud, que consequentemente afetam nossos hands-ons, e, em muitos casos, apenas alguns comandos são necessários atualização/adição. Sendo assim, é crucial e obrigatório, durante a implementação dos projetos, o acompanhamento com a documentação de solução que contém os passos e os comandos necessários e atualizados!

### **Passo 01 - Criar Bucket S3**

- O nome deve ser único e global
- Bucket name: `tcb-aws-bootcamp-<seu nome ou apelido>`
- Region: `us-east-1`

Create Bucket!

### **Passo 02 -** Upload dos arquivos para o bucket

- Total de 5 arquivos.

[index.html]
[style.css]
[tcb.png]
[aws.png]
[rocket.png]

### **Passo 03 -** Criar uma 'Distribution' no CloudFront

- Create CloudFront distribution
- Origin domain: `<YOU-BUCKET-S3>`
- Origin access: `Origin access control settings (recommended)`
- `Create new OAC`: <Necessário efetuar a criação de um novo OAC>

- No fluxo de criação do CloudFront distribution será criada uma politica. Anote/Copie a politica que deverá ser utilizada no passo 4.

Viewer ( Configurar as visões )

- Redirect HTTP to HTTPS
- GET, HEAD (Metodos HTTP que serão utilizados, por ser um site estático)

Web Application Firewall (WAF)

- Do not enable security protections
    - Cuidado ao criar WAF / Web ACL. ( Se criar um WAF, então provavelmente será cobrado valores na AWS)
    - Se for o caso, não esqueça de excluir o WAF e também a regra de ACL criada!

See Price Class

- Use all edge locations (best performance). 
	OBS.: Deste modo a distribuiçaõ do CDN será automático. 
	Após a primeira execução, naquela região, as informações serão espelhadas/replicadas no cache daquela edge location.

Alternate domain name (CNAME).
	OBS.: Automaticamente será criado um DNS para acessar a site. 
	Caso seja necessário um domínio, basta configuar o CNAME e DNS / IPs do dominio.

Default root object: 

- `index.html`

Create distribution

### **Passo 04 - Anexar política gerada pelo CloudFront no Bucket S3**

- Acesse o bucket
- Permissions
- Bucket Policy | Edit
- Colar a política

Save

### **Passo 05 - Acessar o site via CloudFront**

Mas, antes, duas perguntas interessante:

Pergunta 01: `É possível fazer alguma restrição de acesso à aplicação?`

- Acesse a distribuição
- Aba "Security"
- CloudFront geographic restrictions
- Countries - Edit
- Allow List
- Block List
- Geo Block  

Pergunta 02: `Como copiar a política criada pelo CloudFront para anexar no Bucket S3?`

- Acesse a distribuição
- Aba "Origins"
- Selecione a 'origin': bucket S3 / Edit
- Origin access: Origin access control settings (recommended)
- Copy policy

Agora sim, vamos acessar nosso site:

- CloudFront | Distribution | Domain name

Parabéns, você acabou de implementar um site hospedado em um Bucket S3,
com acesso seguro e com baixa latência, usando o Amazon CloudFront,
que distribui o conteúdo de suas aplicações nas edge locations da AWS
espalhadas por todo mundo, e sem acesso direto aos arquivos do site/aplicação.

### **Passo 06 - (opcional) Tente acessar o site via URL do seu arquivo 'index.html'**

- Exemplo: https://tcb-aws-bootcamp-hsp.s3.amazonaws.com/index.html
- AccessDenied

### **Passo 07 - Exclusão dos recursos**

- CloudFront, selecione a 'distribuição', 'Disable'(~ 5 minutos)
    - Acompanhe o Status, quando "Disabled" >> 'Delete';
- CloudFront | Origin access:
    - Control settings - selecione e exclua o “`Origin access control settings`” criado!
- Bucket S3: Empty e Delete!
