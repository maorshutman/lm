# lm
Package lm solves non-linear least squares problems using the Levenberg-Marquardt method.

# Example
* Define a function:
```go
func biggsEXP6Func(dst, x []float64) {
	for i := 0; i < 13; i++ {
		z := float64(i) / 10
		y := math.Exp(-z) - 5*math.Exp(-10*z) + 3*math.Exp(-4*z)
		dst[i] = x[2]*math.Exp(-x[0]*z) - x[3]*math.Exp(-x[1]*z) + x[5]*math.Exp(-x[4]*z) - y
	}
}
```

* Define the jacobian. In this case the jacobian will be evaluated by finite differences:
```go
biggsNumJac := lm.NumJac{Func: biggsEXP6Func}
```

* Call the solver:
```go
biggsProb := lm.LMProblem{
	Dim:        6,
 	Size:       13,
 	Func:       biggsEXP6Func,
 	Jac:        biggsNumJac.Jac,
 	InitParams: []float64{1, 2, 1, 1, 1, 1},
 	Tau:        1e-6,
 	Eps1:       1e-8,
 	Eps2:       1e-8,
}
biggsResults, biggsErr := lm.LM(biggsProb, &lm.Settings{Iterations: 100, ObjectiveTol: 1e-16})
fmt.Println(biggsResults)
fmt.Println(biggsErr)
```

# References
* Madsen, Kaj, Hans Bruun Nielsen, and Ole Tingleff. "Methods for non-linear least squares
problems.", 2nd edition, 2004.
* Lourakis, Manolis. "A Brief Description of the Levenberg-Marquardt Algorithm Implemened 
  by levmar", 2005.
