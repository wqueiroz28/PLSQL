PROBLEMA: PRECISA-SE CONCEDER UM AUMENTO QUE SERÁ PREVIAMENTE INFORMADO A TODO FUNCIONÁRIO QUE GANHAR O MENOR SALÁRIO DA EMPRESA. OS FUNCIONÁRIOS ESTÃO CADASTRADOS E NA MESMA TABELA O ULTIMO SALÁRIO. 
RESOLVENDO: 
1- PODE-SE CRIAR UM PROGRAMA PL-SQL COM AS VARIÁVEIS DE CADA COLUNA QUE SE DESEJA MANIPULAR E CAPTURANDO O PERCENTUAL DE REAJUSTE ATRAVÉS DE UMA VARIÁVEL DE SUBSTITUIÇÃO.  UM CURSOR QUE SELECIONA OS DADOS. DENTRE ESSES DADOS SELECIONA-SE O CODIGO DO FUNCIONARIO E O SALARIO QUE LANÇADOS EM VARIÁVEIS ATRAVÉS DA CAPTURA PELO COMANDO FETCH EM UM LAÇO WHILE OU LOOP  (CASO SEJA O LAÇO FOR NÃO É NECESSÁRIO O CURSOR) POSSIBILITAM A UTILIZAÇÃO DE UM COMANDO UPDATE PARA ATUALIZAR O VALOR CONFORME O LAÇO FOR OCORRENDO.
2- PODE-SE CRIAR UMA STORED PROCEDURE QUE RECEBE UM PARÂMETRO CONTENDO O PERCENTUAL DE REAJUSTE E FAZER A MESMA TAREFA DO PROGRAMA CITADO NO ITEM 1, MAS COM AS VANTAGENS DE UMA PROCEDURE ( SEGURANÇA, DISPENSA DE TRÂNSITO NA REDE, ETC.)
3- PODE-SE CRIAR UMA STORED PROCEDURE EXECUTE UMA PARTE  E QUE CHAME UMA FUNCTION PARA FAZER OUTRA PARTE DA TAREFA, DESDE QUE ISSO DÊ INDEPENDÊNCIA DE UTILIZAÇÃO E POSSA VIR A AUMENTAR A PRODUTIVIDADE.



------------------------------------------------------------------------------------------------------------------------------
----CRIANDO O AMBIENTE PARA EFETUAR OS EXERCICIOS
DROP TABLE DEPT CASCADE CONSTRAINTS;
DROP TABLE EMP CASCADE CONSTRAINTS;
CREATE TABLE DEPT (DEPTNO NUMBER(5) PRIMARY KEY, DNAME VARCHAR2(20));
CREATE TABLE EMP(EMPNO NUMBER(6) PRIMARY KEY, ENAME VARCHAR2(15),SALARIO NUMBER(10,2), deptno number(5) 
references DEPT);
INSERT INTO DEPT VALUES(1,'contabilidade');
INSERT INTO DEPT VALUES(2,'TI');
INSERT INTO DEPT VALUES(3,'VENDAS');
INSERT INTO EMP VALUES(100,'jose',2000,1);
INSERT INTO EMP VALUES(101,'julio',4000,1);
INSERT INTO EMP VALUES(102,'carlos',3000,2);
INSERT INTO EMP VALUES(103,'luiz',1200,2);
INSERT INTO EMP VALUES(104,'Maria',5000,2);
INSERT INTO EMP VALUES(105,'luzia',4500,3);

----------------ABAIXO, CRIAMOS UMA PROCEDURE QUE UTILIZA UMA FUNÇÃO PARA
--------------- ATUALIZAR OS MENORES SALÁRIOS DA EMPRESA, COM UMA TAXA QUE
---------------PODE SER FORNECIDA DIRETYAMENTE COM O COMANDO EXEC OU ATRAVÉS
--------------- DE  UM BLOCO DE CÓDIGO

CREATE OR REPLACE PROCEDURE ATUALIZA_FUNC ( VPERC IN NUMBER)
IS
 VSALARIO NUMBER;
 VEMP NUMBER;
 VNOME VARCHAR2(15);
 VRESULTADO NUMBER(10,2);
CURSOR ATUALIZA IS SELECT EMPNO,ENAME,SALARIO, F_ATUALIZA(EMPNO,SALARIO,VPERC) FROM EMP WHERE SALARIO = (SELECT MIN(SALARIO) FROM EMP);
BEGIN
  OPEN ATUALIZA;
  LOOP
    EXIT WHEN ATUALIZA%NOTFOUND;
    FETCH ATUALIZA INTO VEMP,VNOME,VSALARIO,VRESULTADO;
    DBMS_OUTPUT.PUT_LINE('NUMERO:' || VEMP ||'  '|| 'NOME:'||VNOME ||'  '||'SALARIO:'|| VSALARIO);
    
      END LOOP;
END;
/
