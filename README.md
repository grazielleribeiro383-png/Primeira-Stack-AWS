Desafio DIO: Implementação e Depuração de Stack AWS CloudFormation
Este repositório documenta o processo de tentativa e depuração (debugging) de uma Stack simples de AWS CloudFormation, conforme solicitado pelo desafio da DIO para a criação de um perfil de destaque. O foco foi na infraestrutura como código (IaC) e na resolução de erros comuns de provisionamento.

Objetivos de Aprendizagem
Aplicar conceitos de CloudFormation (.yaml Templates).

Diagnosticar e corrigir falhas de CREATE_FAILED e ROLLBACK_COMPLETE.

Utilizar o GitHub como ferramenta para documentação técnica estruturada.

O que é AWS CloudFormation?
O AWS CloudFormation é um serviço da AWS que permite modelar e provisionar recursos de infraestrutura em nuvem de forma automatizada e repetível, usando arquivos de template (JSON ou YAML). Ele trata a infraestrutura como código (IaC), garantindo consistência e facilitando a gestão dos recursos como uma unidade única (Stack).

O Processo de Implementação
Utilizei o template 01-EC2.yaml fornecido no material do curso e tentei criar a Stack na AWS.

Template Base
O template tinha como objetivo provisionar uma instância EC2 simples com o seguinte código (versão inicial):

Resources:
  MinhaInstancia:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-0ed9277fb7eb570c9
      InstanceType: t2.micro
      Tags :
        - Key: "Name"
          Value: "EC2"

          Insights e Processo de Depuração (Debugging)
Apesar de ser um template simples, foram encontradas falhas sucessivas que exigiram a depuração (debugging) do código e das configurações de região, comprovando a necessidade de testes e validações em IaC.

Etapa,Erro Encontrado,Solução Aplicada,Status Final
1,Error de formato do modelo,Arquivo ZIP subido incorretamente. Solução: Subir o arquivo YAML descompactado (01-EC2.yaml).,OK
2,ImageId does not exist (Em Ohio - us-east-2),A AMI era específica da região Norte da Virgínia (us-east-1). Solução: Mudar a região da AWS para us-east-1.,Falhou (Rollback)
3,Instance type is not eligible for Free Tier,A AWS rejeitou a criação do t2.micro (Plano Gratuito Padrão). Tentativa 1: Mudar InstanceType para t3.micro.,Falhou (Rollback)
4,Instance type is not eligible for Free Tier (Persistente),A restrição de AvailabilityZone e a ImageId desatualizada em conjunto podem causar o erro. Tentativa 2: Remover a restrição AvailabilityZone: us-east-1a e garantir t2.micro com um AMI atualizado.,Falhou (Rollback)

Com certeza! A depuração de erros é uma das habilidades mais importantes na nuvem. Você tem um ótimo material para documentar o processo.Abaixo está o modelo de README.md que foca na sua jornada de depuração e aprendizados, que é o verdadeiro entregável do desafio.Desafio DIO: Implementação e Depuração de Stack AWS CloudFormationEste repositório documenta o processo de tentativa e depuração (debugging) de uma Stack simples de AWS CloudFormation, conforme solicitado pelo desafio da DIO para a criação de um perfil de destaque. O foco foi na infraestrutura como código (IaC) e na resolução de erros comuns de provisionamento.🎯 Objetivos de AprendizagemAplicar conceitos de CloudFormation (.yaml Templates).Diagnosticar e corrigir falhas de CREATE_FAILED e ROLLBACK_COMPLETE.Utilizar o GitHub como ferramenta para documentação técnica estruturada.☁️ O que é AWS CloudFormation?O AWS CloudFormation é um serviço da AWS que permite modelar e provisionar recursos de infraestrutura em nuvem de forma automatizada e repetível, usando arquivos de template (JSON ou YAML). Ele trata a infraestrutura como código (IaC), garantindo consistência e facilitando a gestão dos recursos como uma unidade única (Stack).🚀 O Processo de ImplementaçãoUtilizei o template 01-EC2.yaml fornecido no material do curso e tentei criar a Stack na AWS.📝 Template BaseO template tinha como objetivo provisionar uma instância EC2 simples com o seguinte código (versão inicial):YAMLResources:
  MinhaInstancia:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-0ed9277fb7eb570c9
      InstanceType: t2.micro
      Tags :
        - Key: "Name"
          Value: "EC2"
Insights e Processo de Depuração (Debugging)Apesar de ser um template simples, foram encontradas falhas sucessivas que exigiram a depuração (debugging) do código e das configurações de região, comprovando a necessidade de testes e validações em IaC.EtapaErro EncontradoSolução AplicadaStatus Final1Error de formato do modeloArquivo ZIP subido incorretamente. Solução: Subir o arquivo YAML descompactado (01-EC2.yaml).OK2ImageId does not exist (Em Ohio - us-east-2)A AMI era específica da região Norte da Virgínia (us-east-1). Solução: Mudar a região da AWS para us-east-1.Falhou (Rollback)3Instance type is not eligible for Free TierA AWS rejeitou a criação do t2.micro (Plano Gratuito Padrão). Tentativa 1: Mudar InstanceType para t3.micro.Falhou (Rollback)4Instance type is not eligible for Free Tier (Persistente)A restrição de AvailabilityZone e a ImageId desatualizada em conjunto podem causar o erro. Tentativa 2: Remover a restrição AvailabilityZone: us-east-1a e garantir t2.micro com um AMI atualizado.Falhou (Rollback)Conclusão e Aprendizados TécnicosApesar das múltiplas correções no template (troca de região, ajuste de InstanceType para Free Tier e remoção de AvailabilityZone e atualização da AMI), a criação da pilha não foi concluída.Principal Insight:A falha persistente ("The specified instance type is not eligible for Free Tier") em várias tentativas, mesmo usando tipos de instância t2.micro e t3.micro, indica uma restrição na conta AWS (Service Limit ou Free Tier Allocation) que impediu o provisionamento de recursos. A solução para este erro está fora do escopo do template YAML, exigindo contato com o suporte ou aguardar o ciclo de renovação do Free Tier.
