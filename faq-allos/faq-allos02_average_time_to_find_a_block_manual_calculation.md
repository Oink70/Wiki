[gimmick: math]()

# Question: I'm mining since XYZ with XYZ, why I haven't found a block yet?

$$ Average Time To Find One Block = ( \frac{NetworkHashrate}{LocalHashrate} ) * BlockTime $$

One block = 24 coins (as now)
NetworkHashrate = retrieved by `getmininginfo` command from </>CLI
LocalHashrate = retrieved by `getmininginfo` command
BlockTime = 60 seconds (average)


Real example with - 24 threads Intel Skylake 2.0Ghz -

$$ Average Time To Find One Block = ( \frac{12527490696}{9983712} ) * 60 $$
$$ Average Time To Find One Block = 75.287 seconds (20,91 hours) $$

bear in mind that these are average times to find a block. In real life you may hit a block much sooner or later after finding the last. In the long run it averages out to the values predicted.

(submitted by @TexWiller, edited by Oink.vrsc@)

note: last revision date 2020-02-26
