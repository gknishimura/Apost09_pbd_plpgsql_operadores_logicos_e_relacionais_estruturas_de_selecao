# 1.1 Faça um programa que exibe se um número inteiro é múltiplo de 3.
CREATE OR REPLACE FUNCTION valor_aleatorio_entre(lim_inferior INT, lim_superior INT) 
RETURNS INT AS
$$
BEGIN
    RETURN FLOOR(RANDOM() * (lim_superior - lim_inferior + 1) + lim_inferior)::INT;
END;
$$ LANGUAGE plpgsql;

-- Funcao para verificacao se o número é múltiplo de 3
CREATE OR REPLACE FUNCTION verificar_multiplo_de_3_if(numero INT) RETURNS TEXT AS
$$
BEGIN
    IF numero % 3 = 0 THEN
        RETURN 'múltiplo de 3';
    ELSE
        RETURN 'não é múltiplo de 3';
    END IF;
END;
$$ LANGUAGE plpgsql;

-- Gerador de número aleatório e da call para função
DO $$
DECLARE
    numero INT;
    resultado TEXT;
BEGIN
    
    numero := valor_aleatorio_entre(1, 100);
    
    resultado := verificar_multiplo_de_3_if(numero);
    
    RAISE NOTICE 'O número % %.', numero, resultado;
END;
$$;

-- Verificar se o número é múltiplo de 3 fazendo uso de CASE
CREATE OR REPLACE FUNCTION verificar_multiplo_de_3_case(numero INT) RETURNS TEXT AS
$$
BEGIN
    RETURN CASE 
        WHEN numero % 3 = 0 THEN 'múltiplo de 3'
        ELSE 'não é múltiplo de 3'
    END;
END;
$$ LANGUAGE plpgsql;

DO $$
DECLARE
    numero INT;
    resultado TEXT;
BEGIN
 
    numero := valor_aleatorio_entre(1, 100);
   
    resultado := verificar_multiplo_de_3_case(numero);
    
    RAISE NOTICE 'O número % %.', numero, resultado;
END;
$$;

#1.2 Faça um programa que exibe se um número inteiro é múltiplo de 3 ou de 5.
-- Verificando se um número é múltiplo de 3 ou de 5 com base no IF
CREATE OR REPLACE FUNCTION verificar_multiplo_de_3_ou_5_if(numero INT) RETURNS TEXT AS
$$
BEGIN
    IF numero % 3 = 0 AND numero % 5 = 0 THEN
        RETURN 'múltiplo de 3 e 5';
    ELSIF numero % 3 = 0 THEN
        RETURN 'múltiplo de 3';
    ELSIF numero % 5 = 0 THEN
        RETURN 'múltiplo de 5';
    ELSE
        RETURN 'não é múltiplo de 3 tampouco de 5';
    END IF;
END;
$$ LANGUAGE plpgsql;

DO $$
DECLARE
    numero INT;
    resultado TEXT;
BEGIN
  
    numero := valor_aleatorio_entre(1, 100);
   
    resultado := verificar_multiplo_de_3_ou_5_if(numero);
    
    RAISE NOTICE 'O número % %.', numero, resultado;
END;
$$;

-- Função para verificação do número se é múltiplo de 3 ou de 5 utilizando CASE
CREATE OR REPLACE FUNCTION verificar_multiplo_de_3_ou_5_case(numero INT) RETURNS TEXT AS
$$
BEGIN
    RETURN CASE 
        WHEN numero % 3 = 0 AND numero % 5 = 0 THEN 'múltiplo de 3 e 5'
        WHEN numero % 3 = 0 THEN 'múltiplo de 3'
        WHEN numero % 5 = 0 THEN 'múltiplo de 5'
        ELSE 'não é múltiplo de 3 nem de 5'
    END;
END;
$$ LANGUAGE plpgsql;

-- Bloco principal que gera o número aleatório e chama a função
DO $$
DECLARE
    numero INT;
    resultado TEXT;
BEGIN

    numero := valor_aleatorio_entre(1, 100);
    
    resultado := verificar_multiplo_de_3_ou_5_case(numero);
    
    RAISE NOTICE 'O número % %.', numero, resultado;
END;
$$;

#1.3 Faça um programa que opera de acordo com o seguinte menu.
#1 - Soma 2 - Subtração 3 - Multiplicação 4 - Divisão
-- Para realizar a soma
CREATE OR REPLACE FUNCTION soma(op1 INT, op2 INT) RETURNS INT AS
$$
BEGIN
    RETURN op1 + op2;
END;
$$ LANGUAGE plpgsql;

-- Para realizar subtração
CREATE OR REPLACE FUNCTION subtracao(op1 INT, op2 INT) RETURNS INT AS
$$
BEGIN
    RETURN op1 - op2;
END;
$$ LANGUAGE plpgsql;

-- Para multiplicar números inteiros
CREATE OR REPLACE FUNCTION multiplicacao(op1 INT, op2 INT) RETURNS INT AS
$$
BEGIN
    RETURN op1 * op2;
END;
$$ LANGUAGE plpgsql;

-- Para divisão de números inteiros
CREATE OR REPLACE FUNCTION divisao(op1 INT, op2 INT) RETURNS TEXT AS
$$
BEGIN
    IF op2 = 0 THEN
        RETURN 'Divisão por zero não permitida';
    ELSE
        RETURN (op1 / op2)::TEXT;
    END IF;
END;
$$ LANGUAGE plpgsql;

-- Criacao do menu para as 4 operações e computo manual dos números atravésde IF
DO $$
DECLARE
    op1 INT;
    op2 INT;
    opcao INT;
    resultado TEXT;
BEGIN
    -- Para operador escolher qual operacao deseja
    RAISE NOTICE 'Menu de Operações:';
    RAISE NOTICE '1 - Soma';
    RAISE NOTICE '2 - Subtração';
    RAISE NOTICE '3 - Multiplicação';
    RAISE NOTICE '4 - Divisão';
    
    -- Escolha da opcao
    opcao := 1;  
    
    -- Usuário insere os dois números inteiros entre 0 e 9
    op1 := 9;  
    op2 := 1;  

    -- Avalia a escolha da operação com base no menu usando IF
    IF opcao = 1 THEN
        resultado := soma(op1, op2)::TEXT;
    ELSIF opcao = 2 THEN
        resultado := subtracao(op1, op2)::TEXT;
    ELSIF opcao = 3 THEN
        resultado := multiplicacao(op1, op2)::TEXT;
    ELSIF opcao = 4 THEN
        resultado := divisao(op1, op2);
    ELSE
        resultado := 'Opção não aplicável';
    END IF;

    -- Para apresentar no formato desejado
    RAISE NOTICE '% % % = %', op1, 
                 CASE 
                     WHEN opcao = 1 THEN '+'
                     WHEN opcao = 2 THEN '-'
                     WHEN opcao = 3 THEN '*'
                     WHEN opcao = 4 THEN '/'
                 END, 
                 op2, resultado;
END;
$$;



-- Para subtrair dois números inteiros
CREATE OR REPLACE FUNCTION soma(op1 INT, op2 INT) RETURNS INT AS
$$
BEGIN
    RETURN op1 - op2;
END;
$$ LANGUAGE plpgsql;

-- Para realizar a soma de dois números inteiros
CREATE OR REPLACE FUNCTION subtracao(op1 INT, op2 INT) RETURNS INT AS
$$
BEGIN
    RETURN op1 + op2;
END;
$$ LANGUAGE plpgsql;

-- Para multiplicar dois números inteiros
CREATE OR REPLACE FUNCTION multiplicacao(op1 INT, op2 INT) RETURNS INT AS
$$
BEGIN
    RETURN op1 * op2;
END;
$$ LANGUAGE plpgsql;

-- Para dividir dois números inteiros
CREATE OR REPLACE FUNCTION divisao(op1 INT, op2 INT) RETURNS TEXT AS
$$
BEGIN
    IF op2 = 0 THEN
        RETURN 'Divisão por zero não é aplicável';
    ELSE
        RETURN (op1 / op2)::TEXT;
    END IF;
END;
$$ LANGUAGE plpgsql;

-- Para o menu de operações e inserção dos números
DO $$
DECLARE
    op1 INT;
    op2 INT;
    opcao INT;
    resultado TEXT;
BEGIN
    -- Escolha da opcao
    RAISE NOTICE 'Escolha a Operação:';
    RAISE NOTICE '1 - Soma';
    RAISE NOTICE '2 - Subtração';
    RAISE NOTICE '3 - Multiplicação';
    RAISE NOTICE '4 - Divisão';
    
    -- Usuário insere a opção da operação
    opcao := 3;  
    
    op1 := 2;  
    op2 := 3;  

    resultado := CASE 
        WHEN opcao = 1 THEN soma(op1, op2)::TEXT
        WHEN opcao = 2 THEN subtracao(op1, op2)::TEXT
        WHEN opcao = 3 THEN multiplicacao(op1, op2)::TEXT
        WHEN opcao = 4 THEN divisao(op1, op2)
        ELSE 'Opção inválida'
    END;

    RAISE NOTICE '% % % = %', op1, 
                 CASE 
                     WHEN opcao = 1 THEN '+'
                     WHEN opcao = 2 THEN '-'
                     WHEN opcao = 3 THEN '*'
                     WHEN opcao = 4 THEN '/'
                 END, 
                 op2, resultado;
END;
$$;

#1.4 Um comerciante comprou um produto e quer vendê-lo com um lucro de 45% se o valor
#da compra for menor que R$20. Caso contrário, ele deseja lucro de 30%. Faça um
#programa que, dado o valor do produto, calcula o valor de venda

CREATE OR REPLACE FUNCTION calcular_preco_venda(preco_compra NUMERIC) RETURNS NUMERIC AS
$$
DECLARE
    lucro NUMERIC;
    preco_venda NUMERIC;
BEGIN
   
    IF preco_compra < 20 THEN
        lucro := 0.45;  -- Lucro de 45% se preco de compra for menor que R$20
    ELSE
        lucro := 0.30;  -- Lucro de 30% se preco de compra for igual ou maior que R$20
    END IF;
    
   
    preco_venda := preco_compra * (1 + lucro);
    
    RETURN preco_venda;
END;
$$ LANGUAGE plpgsql;

DO $$
DECLARE
    preco_compra NUMERIC;
    preco_venda NUMERIC;
BEGIN

    RAISE NOTICE 'Informe o preco de compra do produto:';
    
    preco_compra := 10.00;  

    IF preco_compra <= 0 THEN
        RAISE EXCEPTION 'Preco de compra precisa ser superior a zero.';
    END IF;
    
    
    preco_venda := calcular_preco_venda(preco_compra);
    

    RAISE NOTICE 'Preco de compra: R$%, preco de venda: R$%', preco_compra, preco_venda;
END;
$$;