---
layout: post
title:  "Padrão setup em testes no Go"
date:   2023-10-07 12:27:00 -0300
categories: golang testing
---

# Testes e setup sem suite

sobre o que fala?

fala sobre testar uma camada no go em um pacote utilizando uma função de setup encapsulando a função de teste.

```go
func setup( theTest func(*testing.T) ) func(*testing.T){
	return func(t *testing.T) {
        // init mocks
        // ...

        configs = SvcConfig{
            DepOne: depOne,
            DepTwo: depTwo,
        }

        theTest(t)
	}
}

var (
	depOne MockDependency
	depTwo MockDependency

	configs SvcConfig
)

func shouldDoSomething() *gomock.Call {
	return depOne.EXPECT().DoSomething()
}

func shouldDoSomethingDifferent() *gomock.Call {
	return depTwo.EXPECT().DoSomethingDifferent(gomock.Any()).
		Do(func(param SomeKind) {
			// check param`s attributes
		})
}

func shouldDoAnother(expected... OtherKind) *gomock.Call {
	return depTwo.EXPECT().DoAnother(gomock.Any()).
		Do(func(received OtherKind) {
			if len(expected) == 0 || expected[0] != nil {
				return
			}

			expectedOtherKind := expected[0]
			assert.Equal(expectedOtherKind, received)
		})
}

func Test_Service_One(t *testing.T) {
	
	t.Run("should return 200 OK when receive valid info", setup(func(t *testing.T) {
		shouldDoSomething().Return(nil)
		shouldDoSomethingDifferent().Return("fine", nil)
		shouldDoAnother()

		svc := NewSvc(configs)
		svc.Do(/* param */)
	}))

}

```

# Vamos por partes:

## função `setup`

no parametro recebe uma função de teste e o retorna também uma função do mesmo tipo.

No `setup` ja retornamos imediatamente uma função de teste pois dentro dela precisamos da instância do teste para inicializar os mocks.

Em seguida inicializo um objeto de `config` que será utilizado pelo serviço utilizando os mocks.

## funções de comportamento `shouldCall...`

Aqui as funções definidas servem para representar o comportamento esperado do sistema. Aqui vale utilizar temos de negócio para ajudar no entendimento do que o serviço deve fazer e facilitar.

## os testes

O nome do teste indica qual serviço vai ser testado. Dentro dele criamos vários subtests com `t.Run`e é nesse momento que utilizamos o setup.

Dentro do escopo da função do teste todos os mocks estarão instanciados e prontos para serem usados.