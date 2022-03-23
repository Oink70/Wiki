[gimmick: math]()

# Question: I'm mining since XYZ with XYZ, why I haven't found a block yet?

$$ Average Time To Find One Block = ( \frac{NetworkHashrate}{LocalHashrate} ) * BlockTime $$

One block = 12 coins (as now)
NetworkHashrate = retrieved by `getmininginfo` command from </>CLI
LocalHashrate = retrieved by `getmininginfo` command
BlockTime = 60 seconds (average)


Real example with - 31 threads AMD Ryzen 5950x @ 4.4Ghz -

$$ Average Time To Find One Block = ( \frac{851125882237}{46159950} ) * 60 $$
$$ Average Time To Find One Block = 1,106,317 seconds (307 hours or little under 13 days) $$

Bear in mind that these are average times to find a block. In real life you may hit a block much sooner or later after finding the last. In the long run it averages out to the values predicted.

(submitted by @TexWiller, edited by Oink.vrsc@)

note: last revision date 2022-03-23
