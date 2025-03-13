Notes on the intro course from [3b1b](https://www.youtube.com/watch?v=fNk_zzaMoSs). 

$$ \newcommand{\m}[1]{\begin{bmatrix} #1 \end{bmatrix}} $$
----
## I) vector basics

#### what's a vector
- physics: arrows in space, defined by **length** and **direction** and are **movable**
- cs: ordered lists of numbers, i.e. $\m{12.0 \\ 3.0}$
- math: a mix of the above

> *think about an arrow in a coordinate system, starting from the origin*
> \- 3b1b

![[Pasted image 20250311103443.png|300]]
#### fundamental vector operations
##### addition
* each vectors is a step, a movement in space. First, walk $\vec{v}$, then $\vec{w}$.
* this is the same as walking $\vec{v} + \vec{w}$ to start with!
* $\m{1 \\ 2} + \m{3 \\ -1} = \m{1 + 3 \\ 2 + (-1)}$

![[Pasted image 20250311103751.png|300]]
##### multiplying by a scalar
* **scaling**: stretching, squishing, reversing direction. 
* **scalar**: number

![[Pasted image 20250311105003.png|300]]

> [!info] Conclusion
> it doesn't matter whether we fundamentally see a vector as a list of numbers or an arrow in space. What matters is that we can translate between them.

----

## II) linear combinations, span, basis

* $\vec{i}$: unit vector in x direction
* $\vec{j}$: unit vector in y direction
* so what is $\m{3 \\ -2}$ ? Just the scaled $\vec{i}$ and $\vec{j}$ , i.e. $3\vec{i}  + (-2) \vec{j}$
* $\vec{i}$ and $\vec{j}$ are the **basis** of the coordinate system. We could have chosen a different one!
* $3\vec{i}  + (-2) \vec{j}$ is a **linear combination** of those two vectors
* the **span** of the two vectors $\vec{i}$ and $\vec{j}$ is the set of all their linear combinations!
 ![[Pasted image 20250311120636.png|300]]
 
* iow: the span consists of all vectors that can be expressed as a weighted sum of the original vectors, with weights being scalars.
	* in 2d, the span covers the whole plane, except the two vectors are 0 or point in the same direction (then the span covers the line along which they're pointing)
	* in 3d, the *span of the two* vectors is a flat sheet going through 3d space
	* ~ in 3d, the span of three vectors covers 3d space, except some of the vectors are *linearly dependent*
	* if a vector adds a new dimension to the span, they are linearly independent

#### technical definition of **basis**:
> the basis of a vector space is a set of linearly independent vectors that span that space

* linearly independent + span the space
* if not linearly independent, there'd be redundancy, one could just remove a vector
* if not spanning the space, they couldn't form a basis as *not all* vectors could be expressed
* linear independence ensures uniqueness of the representation of a vector in space
* for an n-dimensional vector space, any basis will contain *exactly* n vectors

----
## III) Linear transformations and matrices

**Linear transformation** 
* transformation ~ function
* linear ~ lines remain lines, origin remains fixed (in 2d)
* linear ~ keeping gridlines parallel and evenly spaced
![[Pasted image 20250312142010.png|400]]

> think: arrow a moving into arrow b as a transformation. Then, all arrows moving at once!
> now: vectors as points, not arrows

**How to describe a linear transformation numerically?**

$\m{X_{in} \\ y_{in}} \rightarrow ??? \rightarrow \m{X_{out} \\ y_{out}}$

> [!Note] 
> we only need to record where the basis vector $\hat{i}$ and $\hat{j}$ land, everything else goes from there

![[Pasted image 20250312142402.png|400]]

So more generally, let's say we know where the basis vectors land:

$\hat{i} \rightarrow \m{1 \\-2}$ , $\hat{j} \rightarrow \m{3\\0}$

now where does an arbitrary vector $\m{x\\y}$ land?

$\m{x\\y} \rightarrow x\m{1 \\ -2} + y\m{ 3 \\ 0} \rightarrow \m{ 1x + 3y \\ -2x + 0y }$  

so we can say where any vector lands using this formula! 

> [!note] 
> a 2d linear transformation is completely described by 4 numbers, the two coordinates where $\hat{i}$ lands and the two coordinates where $\hat{j}$ lands!

it's common to package these coordinates into a 2x2 Matrix, where the columns are where $\hat{i}$ lands and where $\hat{j}$ lands! 

Now this (middle bit, it's a sum of scaled vectors!) gives us the intuition for matrix vector multiplication, no need to memorise.

$\m{a \ b \\ c \ d} \m{x \\ y} = x\m{a \\ c} + y\m{ b \\ d} = \m{ax + by \\ cx + dy}$ 

Some insights:
* if the vectors that $\hat{i}$ and $\hat{j}$ land on are linearly dependent, the 2d space collapses into a 1d line

-----

## IV) Matrix multiplications as composition

**Composition**: two linear transformations in a row, like a rotation and a shear. There will be a matrix which represents the rotation, a matrix which represents the shear, and a matrix which represents both together. The latter is where $\hat{i}$ and $\hat{j}$ end up after both transformations. 

> The composition is therefore the **product** of the shear and the rotation matrix.

![[Pasted image 20250313141338.png|400]]
**How to read** : right to left, first rotation, then shear. Comes from function notation $f(g(x))$ .

#### So how to do matrix multiplication with our knowledge so far?

![[Pasted image 20250313142755.png|400]]

* $\hat{i}$ after the $M_1$ transformation lands on $\m{1 \\ 1}$ , $\hat{j}$ lands on $\m{-2 \\ 0}$ . 
* where $\hat{i}$ lands after the $M_2$ transformation: $\m{0 & 2 \\ 1 & 0} \m{1 \\ 1} = 1\m{0 \\ 1} + 1\m{2 \\ 0} = \m{2 \\ 1}$
* where $\hat{j}$ lands after $M_2$ transformation:  $\m{0 & 2 \\ 1 & 0} \m{-2 \\ 0} = -2\m{0 \\ 1} + 0\m{2 \\ 0} = \m{0 \\ -2}$ 
* now just put the two together to get the composite: $\m{2 & 0 \\ 1 & -2}$   

#### Now the general solution, without memorizing:

![[Pasted image 20250313145414.png|400]]

1. after the $M_1$ transformation, $\hat{i}$ lands on $\m{e \\ g}$ 
2. now where does $\m{e \\ g}$ land after the $M_2$ transformation? $e \m{a \\ c} + g \m{b \\ d} = \m{ae + bg \\ ce + dg}$ . Just a recap why that is: It's because $e$ and $g$ just scale where the basis mors land (which is $\m{a \\ c}$ and $\m{b \\ d}$ ). This is because the resulting mor is linearly dependent. 
3. now where does $\m{f \\ h}$ land after the $M_2$ transformation? $f \m{b \\ d} + h \m{b \\ d} = \m{af + bh \\ cf + dh}$ 
4. then, we can just take those together as the first and second column of the composition matrix: $\m{ae + bg & af + bh \\ ce + dg & cf + dh}$ 

> [!Note] What does matrix multiplication really represent?
> Applying one transformation after another.

What this helps with, for example:

* Is $M_1M_2 = M_2M_1$ ? No! when thinking about one transformation, then the next, it becomes clear that it matters which transformation comes first.
* Is matrix multiplication associative? $(AB)C = A(BC)$ ?
	* if one thinks in transformations, $(AB)C$ would be first C, then B, then A. The same is true for $A(BC)$, which is first C, then B, then A! Easy cheesy. 