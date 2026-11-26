
# _*Criação do Repositório referente ao módulo de python voltado ao monitoramento de variáveis operacionais de motores elétricos no ambiente fabril.*_
---

O código inicia nos apresentando o nome do sistema e a versão em que ele se encontra, ele nos revela também os limites pré-programados que iram ditar as regras de funcionamento do sistema, que nos avisará se algo está fora do âmbito normal, se representa perigo ou anormalidade com os seguintes versos:

---

```python
LIMITES = {
    "temperatura_max": 80.0,
    "corrente_max": 18.5,
    "vibracao_max": 4.5,
    "rpm_min": 1400,
    "rpm_max": 1800,
}
```

---

Tendo em vista o código apresentado podemos aferir que ele representa uma maneira trivial de poder analisar os motores industriais de uma empresa de automação industrial. Dentro deles encontramos algumas variáveis que nos ajudam a captar os dados, sendo elas:
1. Nome do motor
2. Setor Responsável
3. Temperatura
4. Corrente
5. Vibração
6. RPM
7. Potência_kw
---
Basicamente esses dados que iram nos auxiliar na coleta e analise sintética dos dados, porém em alguns casos essas variáveis podem ser consideradas falsas:🚫
- Quando algum campo não classificado anteriormente é inserido
- Quando a variável "temperatura" está  < que 0.
- Quando a variável "corrente" está  < que 0.
- Quando a variável "vibração" está  < que 0.
- Quando a variável "rpm" está  < que 0.

---

O Programa também analisa certos dados e avalia o estado dos motores, como por exemplo:

---

```python
def analisar_alertas(motor):
    alertas = []
    if motor["temperatura"] > LIMITES["temperatura_max"]:
        alertas.append(f"Superaquecimento ({motor['temperatura']:.1f} C)")
    if motor["corrente"] > LIMITES["corrente_max"]:
        alertas.append(f"Sobrecorrente ({motor['corrente']:.1f} A)")
    if motor["vibracao"] > LIMITES["vibracao_max"]:
        alertas.append(f"Vibracao elevada ({motor['vibracao']:.1f} mm/s)")
    if motor["rpm"] < LIMITES["rpm_min"] or motor["rpm"] > LIMITES["rpm_max"]:
        alertas.append(f"Rotacao fora da faixa ({motor['rpm']} RPM)")
```

---

Onde dependendo do estado material e a medida das variáveis ele identifica e emite alguns alertas ao operador:
 Status | Alerta 
 :--- | ---:
0 | NORMAL
1 | ATENÇÃO
2...| CRITICO

---

O programa também fornece alguns cálculos que nos auxiliam na analise de dados:

```python
def calcular_estatisticas(motores):
    total = len(motores)
    if total == 0:
        return {}

    temp_media = sum(m["temperatura"] for m in motores) / total
    corrente_media = sum(m["corrente"] for m in motores) / total
    potencia_total = sum(m["potencia_kw"] for m in motores)
    maior_temp = max(motores, key=lambda m: m["temperatura"])

    return {
        "total_motores": total,
        "temperatura_media": round(temp_media, 2),
        "corrente_media": round(corrente_media, 2),
        "potencia_total_kw": round(potencia_total, 2),
        "motor_mais_quente": maior_temp["id"],
    }
```
E por último o programa nos fornece as informações finais revelando todos os dados analisados no período.





