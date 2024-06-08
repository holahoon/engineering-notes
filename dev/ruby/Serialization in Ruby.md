We deconstruct objects or data structures in a format that enables easy storage/transfer and reconstruct them whenever required. This is serialization.

Ruby provides two different mechanisms to send data to another program across the web or save Ruby objects in a files:
1. Binary
2. Human -Readable
	- YAML
	- JSOn

## 1. Binary Format

Ruby supports binary serialization through the `Marshal` module [available in its standard library](https://ruby-doc.org/core-2.6.3/Marshal.html).
The marshalling library transforms the collection of Ruby objects into a stream of bytes that we humans can't decipher but Ruby can. The `Marshal.dump` method is used to convert the object to a byte stream and the `Marshal.load or Marshal.restore` method reconstructs the object.

Here's an example:
```ruby
class Furniture
  def initialize(category, primary_material)
    @category = category
    @primary_material = primary_material
  end

  def to_s
    "Category: #{@category} \nMaterial: #{@primary_material}\n"
  end
end

class WoodenBed < Furniture
  def initialize(size, color)
    super('Bed', 'Wooden')
    @size = size
  end

  def to_s
    "#{super} Size: #{@size} \n"
  end
end

class KingSizeWoodenBed < WoodenBed
  def initialize(height, width)
    super('King Size', 'Black')
    @height = height
    @width = width
  end

  def to_s
    "#{super}  Height: #{@height} \n  Width: #{@width} \n"
  end
end
```

Here's creating an object of the above class and serialize it using Marsha.
```ruby
time = Benchmark.measure do
  selected_bed = KingSizeWoodenBed.new(76, 80)

  print "Original object \n\n"
  puts selected_bed

  serialized_object = Marshal::dump(selected_bed)

  print "\nSerialized object\n\n"
  puts serialized_object

  selected_bed = Marshal::load(serialized_object)

  print "\n\nOriginal object back\n\n"
  puts selected_bed
end

puts "\n\n Time taken: #{time}"
```

Output:
```ruby
Original object 

Category: Bed 
Material: Wooden
 Size: King Size 
  Height: 76 
  Width: 80 

Serialized object

o:KingSizeWoodenBed
:@categoryIBed:ET:@primary_materialI"
                                        Wooden;T:
@sizeI"King Size;T:
                    @heightiQ:
                              @widthiU


Original object back

Category: Bed 
Material: Wooden
 Size: King Size 
  Height: 76 
  Width: 80 


 Time taken:   0.000128   0.000066   0.000194 (  0.000189)
```

As you can see, even though the encoded string looks like gibberish, the reconstructed string is the same as the original. This type of serialization can be used when we are not concerned with being able to read the encoded data.  
Note that it took overall `0.19` ms.

Take a look at this particle for the rest: https://blog.kiprosh.com/serialization_in_ruby_on_rails_part_one/

