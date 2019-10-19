---
layout: post
title: Utiliza?o do Mockito em Android
description: Revis? de itens utilizados em um projeto Android
author: Camila L. Oliveira
---

Testes unit?ios servem para testar a implementa?o de um m?odo e de seus componentes em uma aplica?o Android. Eles n? te garantem que o software funciona em todos os cen?ios poss?eis, mas te permitem assegurar que funcionam em cen?ios espec?icos definidos previamente. Instabilidades de rede e de servi?s, respostas diferentes em requisi?es ao servidor, tudo isso dificulta muito a escrita dos testes. Existem algumas ferramentas que te ajudam a configurar esse ambiente controlado e previs?el para que possamos validar os fluxos (SANTOS, 2018).

Existem duas formas para fazer estes testes unit?ios: por meio do jUnit (automaticamente implementado quando se cria uma aplica?o android e, atrav? do Mockito, aonde podemos tamb? testar as implementa?es de nossas classes e activities criadas no projeto

---
## Junit 

### Implementa?o

Em build.gradle(app)
<pre><code>
dependencies {
  
  // outras depend?cias da aplica?o
  // adicionar essa linha
  testImplementation 'junit:junit:4.12'

}
</code></pre>

### Testando o m?odo de soma

Em TestClass.java
<pre><code>
@Test 
public void plusOperationTest() {
  assertEquals(4, sum(2, 2));
}
</code></pre>

Em TestClass.kt
<pre><code>
@Test 
fun plusOperationTest() {
  assertEquals(4, sum(2, 2))
}
</code></pre>

---
## Mockito

### Implementa?o

Em build.gradle(app)
<pre><code>
dependencies {
  // outras depend?cias da aplica?o
  // Depend?cia principal do Mockito
  testImplementation 'org.mockito:mockito-core:2.28.2'
  // Depend?cia do Mockito para testes no Android
  androidTestImplementation 'org.mockito:mockito-android:2.28.2'
  // Depend?cia do Mockito para ser poss?el mockar classes e m?odos constantes
  testImplementation 'org.mockito:mockito-inline:2.28.2'
}
</code></pre>

### Exemplo de Teste de Implementa?o

Estamos criando, neste exemplo abaixo, uma aplica?o que vai medir o consumo do combust?el (seja gasolina, etanol, g? veicular), a dist?cia percorrida para determinado lugar e a autonomia (ou seja, a quilometragem) do ve?ulo.
O exemplo conter?uma MVVM, logo, seu benef?io ser?l?ica de neg?io de cada tela fica no ViewModel uma classe que ?respons?el, por ligar a interface com os dados utilizados na aplica?o, com o uso desse modelo fica mais f?il escrever testes unit?ios no projeto, j?que dividimos as responsabilidades entre a interface (View), l?ica de neg?io (ViewModel) e a representa?o dos dados utilizados no seu projeto (Model).
Caso precise de uma revis? de MVVM, este [artigo](https://medium.com/@soutoss/arquiteturas-em-android-mvvm-kotlin-retrofit-parte-1-2ac77c8a26) ir?lhes ajudar.

---
### Importante
O exemplo conter?o c?igo da ViewModel em Kotlin.
Lembre-se de fazer as demais partes de sua aplica?o.
Caso deseja fazer em Java, cole o conte?do e pe? ao Android Studio a convers?.
---

Em MainActivityViewModel.kt
<pre><code>
package com.example.mytrip.viewmodels

import android.app.Application
import android.arch.lifecycle.AndroidViewModel
import android.databinding.ObservableField
import android.widget.Toast

class MainActivityViewModel(application: Application) : AndroidViewModel(application) {
  
    // Valores dos EditText usando DataBinding

    public val distancia: ObservableField<String> = ObservableField()
    public val preco: ObservableField<String> = ObservableField()
    public val autonomia: ObservableField<String> = ObservableField()

    val resultado: ObservableField<String> = ObservableField()
  
    val errorInformation : MutableLiveData<String> = MutableLiveData<String>()

    fun handleCalculateButtonClick() {
        handleCalculate()
    }

    /**
     * Fun?o respons?el por realizar o c?culo dos gastos com a viagem
     * Baseado na dist?cia percorrida * pre? m?io do combust?el / pela autonomia do ve?ulo
     */

    fun handleCalculate() {
	when {
		isValid()->{
			try {
	        	        // (distancia * pre?) / autonomia
        	        	val distance = distancia?.get().toString().toFloat()
		                val price = preco?.get().toString().toFloat()
                		val autonomy = autonomia?.get().toString().toFloat()

		                // Realiza o c?culo ((distancia * pre?) / autonomia)
                		val result : Float = ((distance * price) / autonomy)
		                // Seta o valor calculado na tela
                		resultado.set("Total: R$ $result")
	            	} catch (nfe: NumberFormatException) {
        	        	// Caso ocorra erro de convers? num?ica, solicita ao usu?io para preencher com valores v?idos              
                		Toast.makeText(getApplication(), "Por Favor Informe valores v?idos", Toast.LENGTH_LONG).show()                         
            		}
		} else ->{
			// Caso n? tenha preenchido todos os campos, solicita o preenchimento          
            		Toast.makeText(getApplication(), "Por Favor Informe valores v?idos", Toast.LENGTH_LONG).show()
		}
	}
    }

    /**
     * Verifica se todos os campos foram preenchidos
     */

    fun isValid(): Boolean {
        return distancia?.get() != ""
                && preco?.get() != ""
                && autonomia?.get() != ""
                && autonomia?.get() != "0"

    }
  
  
    fun getError(): LiveData<String> = errorInformation
  
}
</code></pre>

## O que isto significa?
Significa que minha activity j?possui uma ViewModel, que est?sendo implementada em minha classe da activity. Os valores dos inputs s? utilizados por meio do DataBinding, que tem o objetivo de prover uma conex? entre a classe e o layout da activity, logo evitando o findViewById() e facilitando na utiliza?o da fun?o handleCalculateButtonClick diretamente da declara?o do bot? XML.

Criando o layout da MainActivityViewModel
<pre><code>

<?xml version="1.0" encoding="utf-8"?>
<!--Cabe?lho do arquivo XML padr? do Android Studio na linha 1 -->

<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
                name="viewModel"
                type="com.example.mytrip.viewmodels.MainActivityViewModel" />
    </data>

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/colorLightGray">
    
    <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

      <!-- Componentes Aqui -->
      <Button
            android:id="@+id/buttonCalculate"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_margin="15dp"
            android:padding="20dp"
            android:text="@string/calcular"
            <!-- Chamada da fun?o diretamente pelo XML -->
            android:onClick="@{() ->     viewModel.handleCalculateButtonClick()}"
            android:background="@drawable/round_btn"
            android:textAllCaps="true"
            android:textColor="@color/colorLightGray"
            android:textSize="20sp" />
      <!-- Mais Alguns Componentes Aqui -->
    </LinearLayout>
   </ScrollView>
</layout>
</code></pre>

Agora, vamos criar o teste de nossa activity, para tanto, voc?precisa criar o arquivo de teste em src/test (ou **caminhodoseuprojeto**(test)).

Criando MainActivityViewModelTest.kt
<pre><code>
package com.example.mytrip

import android.app.Application
import android.databinding.ObservableField
import android.util.Log
import com.example.mytrip.viewmodels.MainActivityViewModel
import org.junit.Assert
import org.junit.Before
import org.junit.Test
import org.mockito.Mockito
import org.mockito.MockitoAnnotations
import org.junit.Assert.*
import org.mockito.Mockito.*

class MainActivityViewModelTest {


/**
* fun?o setUp ?respons?el por inicializar os mocks do Mockito em cada
* teste que estiver nesse classe, e a annotation Before ?quem d?o poder
* para a fun?o fazer isso, toda fun?o que estiver com essa marca?o vai
* ser executada, antes de cada teste que estiver na classe de teste.
*/

    @Before
    fun setUp() {
        MockitoAnnotations.initMocks(this)
    }

/**
* isValidReturnTrueTest() valida caso estamos,
* preenchendo corretamente os inputs do aplicativo,
* sem deixar nenhum vazio, e com o valor da autonomia
* maior que zero, assim garantindo que posteriormente
* o c?culo do combust?el, vai poder ser feito sem problemas.
*/
    @Test
    fun isValidReturnTrueTest() {

        val mainActivityViewModel = MainActivityViewModel(Mockito.mock(Application::class.java))

        val distancia = mainActivityViewModel.distancia
        distancia.set("100")

        val preco = mainActivityViewModel.preco
        preco.set("30")

        val autonomia = mainActivityViewModel.autonomia
        autonomia.set("15")


        assertTrue(mainActivityViewModel.isValid())

    }

/**
* isValidReturnFalseTest() ?feito o fluxo
* ao contr?io do anterior, que ?quando o
* usu?io n? preencheu os campos. Assim
* tamb? ?necess?io instanciar o ViewModel
* e atribuir o valor vazio para os inputs,
* e nesse caso utilizar a fun?o assertFalse()
* do JUnit que ?igual, a fun?o que usamos
* anteriormente, mas valida caso o valor passado
* ?igual a false, assim a fun?o isValid() vai
* retornar false, e o teste ir?passar sem problemas.
*/

    @Test
    fun isValidReturnFalseTest() {

        val mainActivityViewModel = MainActivityViewModel(Mockito.mock(Application::class.java))

        val distancia = mainActivityViewModel.distancia
        distancia.set("")

        val preco = mainActivityViewModel.preco
        preco.set("")

        val autonomia = mainActivityViewModel.autonomia
        autonomia.set("")

        assertFalse(mainActivityViewModel.isValid())

    }

/**
* ValidHandleCalculation() ele ?respons?el por testar caso o c?culo do combust?el est?sendo feito da maneira esperada, nele s? preenchidos todos os inputs corretamente, e o m?odo handleCalculate ?chamado, onde o c?culo ?feito, ap? isso o valor do resultado do c?culo, ?obtido atrav? da vari?el resultado que est?ligada ao TextView que mostra esse valor no aplicativo utilizando o DataBinding no ViewModel.
*/

    @Test
    fun ValidHandleCalculation() {

        val mainActivityViewModel = MainActivityViewModel(Mockito.mock(Application::class.java))

        val distancia = mainActivityViewModel.distancia
        distancia.set("30")

        val preco = mainActivityViewModel.preco
        preco.set("15")

        val autonomia = mainActivityViewModel.autonomia
        autonomia.set("250")

        val resultado = mainActivityViewModel.resultado

        mainActivityViewModel.handleCalculate()

        assertEquals("Total: R$ 1.8", resultado.get())
    }

    @Test
    fun ValidHandleCalculationWrongNumber() {

        val mainActivityViewModel = MainActivityViewModel(Mockito.mock(Application::class.java))

        val distancia = mainActivityViewModel.distancia
        distancia.set("asadsaddsasdsdadssd")

        val preco = mainActivityViewModel.preco
        preco.set("15")

        val autonomia = mainActivityViewModel.autonomia
        autonomia.set("250")

        val resultado = mainActivityViewModel.resultado

        var numberFormatException = false

        try {
            mainActivityViewModel.handleCalculate()
            numberFormatException = false
        } catch (e: Exception) {
            numberFormatException = true
        }
        assertTrue(numberFormatException)
    }

    @Test
    fun handleCalculateButtonClickCallHandleCalculate() {
        // Test the handleCalculateButtonClick call HandleCalculate

        val mainActivityViewModel = MainActivityViewModel(Mockito.mock(Application::class.java))

        val distancia = mainActivityViewModel.distancia
        distancia.set("30")

        val preco = mainActivityViewModel.preco
        preco.set("15")

        val autonomia = mainActivityViewModel.autonomia
        autonomia.set("250")

        val spy = spy(mainActivityViewModel)
        spy.handleCalculateButtonClick()

        verify(spy, Mockito.times(1)).handleCalculate()
    }
}
</code></pre>


---
## +
Refer?cias:
[tadeumx1](https://medium.com/@tadeumx1/introdu%C3%A7%C3%A3o-a-testes-unit%C3%A1rios-com-junit-e-mockito-no-android-67104232b878) no Medium
[Diego Santos](https://dextra.com.br/pt/ambiente-controlado-para-testes-parte-1-mockito-e-injecao-de-dependencia/) no DExtra
