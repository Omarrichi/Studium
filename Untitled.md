Gegenbeispiel:
Sei $\sigma := \{c,d\}, \Gamma := \{c \dot{=} c\}, \Delta :=\emptyset, \theta := c, \eta := d \text{ und } \varphi:= x \dot{=} c$

Offensichtlich sind die Prämisse gültig da, $c \dot{=} c \vdash c \dot{=} d, c \dot{=} c$
Aber die Konklusion ist nicht gültig da, $c \dot{=} c \vdash c \dot{=} d, d \dot{=} c$

Sei $\mathfrak{A}= \{\{1,2\}, c^{\mathfrak{A}} := 1, d^{\mathfrak{A}} :=2\}$ und $\mathfrak{b}:$ Var $\to A$ eine Belegung mit $\mathfrak{b}(x) =1$. Die anderen Variablen sind beliebig. Dann ist $(\mathfrak{A}, \mathfrak{b}) \models \Gamma$ aber $(\mathfrak{A}, \mathfrak{b}) \nvDash  \theta \dot{=}  \eta ,\varphi \frac{\eta}{x}$ Denn,

$(\mathfrak{A}, \mathfrak{b}) \models c \dot{=} c$, aber $(\mathfrak{A}, \mathfrak{b}) \nvDash c \dot{=} d, \ddot{}$