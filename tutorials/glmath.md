The GLMath library, part of the HTML 3D library, is a collection
of math functions for working with vectors, matrices, and quaternions.

Here is an overview of these three data types.

Vectors
------------

A vector is simply a set of 3 or 4 elements that are related
to each other.  As such, a vector can symbolize a position, a direction,
a ray, a color, or anything else.

If a vector describes a position, direction,
or normal, the four elements are given as X, Y, Z, and W, in that order.
If a vector describes a color, the four elements are given as red, green,
blue, and alpha, in that order (where each element ranges from 0-1).
The methods in this class treat arrays as vectors.  Functions dealing
with vectors begin with "vec".

Matrices
------------

A matrix is a 16- or 9-element array that describes a
transformation from one coordinate system to another. Transformations
include translation (shifting), scaling, and rotation.
Functions dealing with matrices begin with "mat".

All functions dealing with 4x4 matrices assume that
the translation elements in x, y, and z are located in the
c (zero-based indices 12, 13, and 14).  In mathematical publications,
matrices are often notated in column-major order, in which each
element of the matrix is placed in columns, rather than in rows, as in the following example:

<math>
<mfenced open="[" close="]">
 <mtable>
 <mtr>
 <mtd><mi>matrix[0]</mi></mtd>
 <mtd><mi>matrix[4]</mi></mtd>
 <mtd><mi>matrix[8]</mi></mtd>
 <mtd><mi>matrix[12]</mi></mtd>
 </mtr>
 <mtr>
 <mtd><mi>matrix[1]</mi></mtd>
 <mtd><mi>matrix[5]</mi></mtd>
 <mtd><mi>matrix[9]</mi></mtd>
 <mtd><mi>matrix[13]</mi></mtd>
 </mtr>
 <mtr>
 <mtd><mi>matrix[2]</mi></mtd>
 <mtd><mi>matrix[6]</mi></mtd>
 <mtd><mi>matrix[10]</mi></mtd>
 <mtd><mi>matrix[14]</mi></mtd>
 </mtr>
 <mtr>
 <mtd><mi>matrix[3]</mi></mtd>
 <mtd><mi>matrix[7]</mi></mtd>
 <mtd><mi>matrix[11]</mi></mtd>
 <mtd><mi>matrix[15]</mi></mtd>
 </mtr>
</mtable>
</mfenced>
</math>

The numbers in brackets in the matrix above are the zero-based indices
into the matrix arrays passed to GLMath's matrix methods.

All functions dealing with matrices also assume
a right-handed coordinate system (such as OpenGL's), in which the z-axis points
toward (not away from) the viewer whenever the x-axis points to
the right and the y-axis points up.

**Transforming Points**

The transformation formula multiplies a matrix by a 3D point to change that point's
position:

* output.x = matrix[0] &times; input.x + matrix[4] &times; input.y + matrix[8] &times; input.z + matrix[12]
* output.y = matrix[1] &times; input.x + matrix[5] &times; input.y + matrix[9] &times; input.z + matrix[13]
* output.z = matrix[2] &times; input.x + matrix[6] &times; input.y + matrix[10] &times; input.z + matrix[14]

Note that:

* For the purpose of transforming 3D points, the last row of the matrix above can be ignored,
since it doesn't transform x, y, or z. (It would correspond to the W component of the
output.)
* To make the formula equivalent to multiplying by a 4-component vector, set the input
vector's W component to 1.

The following sections describe different kinds of matrix transformations in more detail.

Related functions:

* [GLMath.mat4transform()]{@link glmath.GLMath.mat4transform} -
 Transforms a 4-element vector with a 4x4 matrix
* [GLMath.mat3transform()]{@link glmath.GLMath.mat3transform} -
 Transforms a 3-element vector with a 3x3 matrix

**Translation**

A translation is a shifting of an object's position.  In a transformation matrix,
this shifting effectively happens after all other transformations such as scaling and rotation.
It uses the 13th, 14th, and 15th elements of the matrix as seen here:

<math>
<mfenced open="[" close="]">
 <mtable>
 <mtr>
 <mtd><mn>1</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>tx</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>1</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>ty</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>1</mn></mtd>
 <mtd><mn>tz</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mn>1</mn></mtd>
 </mtr>
</mtable>
</mfenced>
</math>

where `tx` is added to the x-coordinate, `ty` is added to the y-coordinate, and
`tz` is added to the z-coordinate.  The transformation formulas would look like:

* output.x = 1 &times; input.x + 0 &times; input.y + 0 &times; input.z + tx
* output.y = 0 &times; input.x + 1 &times; input.y + 0 &times; input.z + ty
* output.z = 0 &times; input.x + 0 &times; input.y + 1 &times; input.z + tz

For example, we add the input x and `tx` to get the output x, and likewise for
y and z.

Related functions:

* [GLMath.mat4translated()]{@link glmath.GLMath.mat4translated} -
 Returns a translation matrix
* [GLMath.mat4translate()]{@link glmath.GLMath.mat4translate} -
 Multiplies a matrix by a translation.

**Scaling**

Scaling changes an object's size.  Scaling uses the 1st,
6th, and 11th elements of the matrix as seen here:

<math>
<mfenced open="[" close="]">
 <mtable>
 <mtr>
 <mtd><mi>sx</mi></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mi>sy</mi></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mi>sz</mi></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mn>1</mn></mtd>
 </mtr>
</mtable>
</mfenced>
</math>

where the x-coordinate is multiplied by `sx`, the y-coordinate is multiplied by `sy`, and
the z-coordinate is multiplied by `sz`.

The scaling formula would look like:

* output.x = sx &times; input.x + 0 &times; input.y + 0 &times; input.z + 0
* output.y = 0 &times; input.x + sy &times; input.y + 0 &times; input.z + 0
* output.z = 0 &times; input.x + 0 &times; input.y + sz &times; input.z + 0

For example, we multiply the input x by `sx` to get the output x.  If `sx` is 1, x
remains unchanged.  Likewise for y and z.

If `sx`, `sy`, or `sz` is -1, that coordinate is _reflected_ along the corresponding axis.

If `sx`, `sy`, and `sz` are all 1, we have an _identity matrix_, where the input vector
is equal to the output vector.

Related functions:

* [GLMath.mat4identity()]{@link glmath.GLMath.mat4identity} -
 Returns a 4x4 identity matrix
* [GLMath.mat3identity()]{@link glmath.GLMath.mat3identity} -
 Returns a 3x3 identity matrix
* [GLMath.mat4scaled()]{@link glmath.GLMath.mat4scaled} -
 Returns a scaling matrix
* [GLMath.mat4scale()]{@link glmath.GLMath.mat4scale} -
 Multiplies a matrix by a scaling.
* [GLMath.mat4scaleInPlace()]{@link glmath.GLMath.mat4scaleInPlace} -
 Multiplies a matrix in place by a scaling.

**Rotation**

Rotation changes an object's orientation.  Rotation uses the upper-left
corner of a matrix.  Given an angle of rotation, &theta; (in radians; multiply
degrees by &pi; and divide by 180), the transformation matrix is as follows.

<figure>
<math>
<mfenced open="[" close="]">
 <mtable>
 <mtr>
 <mtd><mn>1</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mi>cos</mi><mi>&theta;</mi></mtd>
 <mtd><mo>-</mo><mi>sin</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mi>sin</mi><mi>&theta;</mi></mtd>
 <mtd><mi>cos</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mn>1</mn></mtd>
 </mtr>
</mtable>
</mfenced>
</math>
<figcaption>Rotation about the X axis.</figcaption></figure>
<figure>
<math>
<mfenced open="[" close="]">
 <mtable>
 <mtr>
 <mtd><mi>cos</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mi>sin</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>1</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mo>-</mo><mi>sin</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mi>cos</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mn>1</mn></mtd>
 </mtr>
</mtable>
</mfenced>
</math>
<figcaption>Rotation about the Y axis.</figcaption></figure>
<figure>
<math>
<mfenced open="[" close="]">
 <mtable>
 <mtr>
 <mtd><mi>cos</mi><mi>&theta;</mi></mtd>
 <mtd><mo>-</mo><mi>sin</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mi>sin</mi><mi>&theta;</mi></mtd>
 <mtd><mi>cos</mi><mi>&theta;</mi></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>0</mn></mtd>
 <mtd><mn>1</mn></mtd>
 <mtd><mn>0</mn></mtd>
 </mtr>
 <mtr>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mi>0</mi></mtd>
 <mtd><mn>1</mn></mtd>
 </mtr>
</mtable>
</mfenced>
</math>
<figcaption>Rotation about the Z axis.</figcaption></figure>

(Note: GLMath assumes a coordinate system in which the Y axis points up
by default.  In coordinate systems where the Y axis points down by default,
the rotation matrices will differ.)

For conciseness, we only give the rotation formula for the X axis,
which would look like:

* output.x = 1 &times; input.x + 0 &times; input.y + 0 &times; input.z + 0
* output.y = 0 &times; input.x + (cos &theta;) &times; input.y + -(sin &theta;) &times; input.z + 0
* output.z = 0 &times; input.x + (sin &theta;) &times; input.y + (cos &theta;) &times; input.z + 0

Note that when we rotate a point about the X axis, the X coordinate is unchanged
and the Y and Z coordinates are adjusted in the rotation.  For rotations about the
Y axis or the Z axis, the Y or Z coordinate, respectively, is likewise unchanged.

As an example, say we rotate 60 degrees about the X axis (`mat4rotated(60, 1, 0, 0)`,
&theta; = 60).
We calculate <i>cos &theta;</i> as 0.5 and <i>sin &theta;</i> as about 0.866025.
We plug those numbers into the rotation formula to get a formula for rotating a
point 60 degrees about the X axis.

* output.x = 1 &times; input.x + 0 &times; input.y + 0 &times; input.z + 0 = input.x
* output.y ~= 0 &times; input.x + 0.5 &times; input.y + -0.866025 &times; input.z + 0
* output.z ~= 0 &times; input.x + 0.866025 &times; input.y + 0.5 &times; input.z + 0

If a point is located at (10, 20, 30), the rotated point would now be:

* output.x = 1 &times; 10 + 0 &times; 20 + 0 &times; 30 + 0
* = 1 &times; 10
* = 10
* output.y ~= 0 &times; 10 + 0.5 &times; 20 + -0.866025 &times; 30 + 0
* ~= 0.5 &times; 20 + -0.866025 &times; 30
* ~= 10 + -25.98075
* ~= -15.98075
* output.z ~= 0 &times; 10 + 0.866025 &times; 20 + 0.5 &times; 30 + 0
* ~= 0.866025 &times; 20 + 0.5 &times; 30
* ~= 17.3205 + 15
* ~= 32.3205

So the rotated point would be at about (10, -15.98075, 32.3205).

Related functions:

* [GLMath.mat4rotated()]{@link glmath.GLMath.mat4rotated} -
 Returns a translation matrix
* [GLMath.mat4rotate()]{@link glmath.GLMath.mat4rotate} -
 Multiplies a matrix by a translation.

**Matrix Multiplication**

Two matrices can be combined into a single transformation. To do so,
the matrices are multiplied such that the transformations
they describe happen in reverse order.  For example, if the first matrix
(input matrix) describes a translation and the second
matrix describes a scaling, the multiplied matrix will
describe the effect of scaling then translation.

Matrix multiplication is not commutative; the order
of multiplying matrices is important.  This multiplication
behavior in GLMath follows that of OpenGL and is opposite to that in the
D3DX and DirectXMath libraries.

The methods mat4multiply, mat4scale, mat4scaleInPlace, mat4translate, and
mat4rotate involve multiplying 4x4 matrices

Related functions:

* [GLMath.mat4multiply()]{@link glmath.GLMath.mat4multiply} -
 Multiplies two matrices

**Other Transformations**

In all the transformations described above, the last row in the transformation matrix is
(0, 0, 0, 1).  However, this is not the case for
some transformations in the GLMath library.

One example of such a transformation is the frustum matrix.  When a 4-element
vector is transformed with this matrix, its W component is generated as follows:

* output.w = 0 &times; input.x + 0 &times; input.y + -1 &times; input.z + 0

The W component of the transformed vector is used to help determine the "depth" of
the transformed vector.

Related functions:

* [GLMath.mat4frustum()]{@link glmath.GLMath.mat4frustum} -
 Returns a frustum matrix
* [GLMath.mat4perspective()]{@link glmath.GLMath.mat4perspective} -
 Returns a field-of-view perspective matrix

Quaternions
--------------

A quaternion is a 4-element array that describes a
3D rotation.  The first three elements are the X, Y, and Z components
(axis of rotation multiplied by the sine of half the angle)
and the fourth component is the W component (cosine of half the angle).
Functions dealing with quaternions begin with "quat".

Euler angles (pitch, yaw, and roll angles) can also be used to describe 3D
rotations, but they have disadvantages:

* In general, the order of Euler angle rotations is important. For example, a 30-degree
pitch followed by a 20-degree roll is not the same as a 20-degree pitch followed
by a 30-degree roll.
* Euler angle rotations are not easily reversible when the same order
of angles is used. For example, a negative 30-degree
pitch followed by a negative 20-degree roll does not undo a 30-degree
pitch followed by a 20-degree roll.

The methods quatMultiply, quatFromPitchYawRoll, among others, involve
multiplying quaternions, combining multiple rotations into a single
rotation.  In these methods, multiplying one rotation by another
creates a combined rotation in which the second rotation happens
before the first rotation.  Like matrix multiplication, quaternion
multiplication is not commutative. This multiplication behavior
is opposite to that in the D3DX and DirectXMath libraries.