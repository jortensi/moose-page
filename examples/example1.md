[gimmick: math]()

<script type="text/javascript">
$.get('test/ex01.i', function(data) {
        $("#msgid").text(data);
        var lines = data.split('\n')
        var output = '';
        for (var i = 0, len = lines.length; i < len; i++){
	var l = $.trim($(lines[i]).text());
	if ( l == '[Variables]' ) {
	  alert(l)
	  output += l
	}  
	} 
        $("#varid").text(output) 	
    }, "text");
</script>

Example 1: As Simple As It Gets
===============================
It is possible to solve a simple diffusion problem with MOOSE without adding any source code, all that is necessary is an input file. This input file must contain a minimum set of input file blocks: Mesh, Variables, Kernels, BCs, Executioner, and Output.

Mesh Block
----------
First, you need a mesh. Here a mesh if being read from an ExodusII file named mug.e.

```
[Mesh]
  file = mug.e
[]
```

Variables Block
---------------
The problem is going to solve for a variable that we arbitrarily are naming 'diffused'. This variable will be solved for using linear Lagrange finite elements.
<pre><code id="varid"></code></pre>

```
[Variables]
  active = 'diffused'
  [./diffused]
    order = FIRST
    family = LAGRANGE
  [../]
[]
```

Kernels Block
-------------
The equation being solve for this example is a diffusion problem, with the weak form defined as
$$ (-\nabla \cdot \nabla u, \psi_i) = 0. $$
A Kernel for this equation already exists within MOOSE: Diffusion. Thus, simply create a Kernel block to utilize this object (see \ref kernels Section for additional details). This block specifies the type to be the aforementioned built-in Diffusion object and the variable to be the 'diffused' variable created in the \ref ex01_variables.
```
[Kernels]
  active = 'diff'
  [./diff]
    type = Diffusion
    variable = diffused
  [../]
[]
```

Program Execution
-----------------
```
make -j8
./ex01-opt -i ex01.i
```

Input File: ex01.i
------------------
<pre><code id="msgid"></code></pre>
