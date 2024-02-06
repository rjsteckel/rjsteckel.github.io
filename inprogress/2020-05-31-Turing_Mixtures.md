@def title = "Mixtures with Turing"

Probabilistic programming seems to get more and more interest each year, but has yet to really take off. There's been Church, Anglican, Stan, Edward, Tensorflow probability, Gen and Turing. To me, Turing seems the most intuitive and natural feeling. It feels like I can easily take a generative model, found in a paper, and write it in Turing's model sytax.

```
using Turing
using Distributions
using StatsPlots

CD = Categorical([.3, .5, .2])
PD = [Normal(-1), Normal(3), Normal(8)]
N = 100
y = Vector{Float32}()
for i in 1:N
    c = rand(CD)
    v = rand(PD[c])
    push!(y, v)
end

histogram(y, bins=50)


@model mm(y) = begin
    N = length(y)

    μ1 ~ Normal()
    μ2 ~ Normal()
    μ3 ~ Normal()

    # μ ~ [μ1, μ2, μ3]  #***
    μ = [μ1, μ2, μ3]
    ps ~ Dirichlet(ones(3))

    k = Vector{Int}(undef, N)
    for i in 1:N
        k[i] ~ Categorical(ps)
        y[i] ~ Normal(μ[k[i]])
    end
   return k
end


s = Gibbs(PG(100, :k), HMC(.1, 10, :μ1, :μ2, :μ3, :ps))
chn = sample(mm(y), s, 100)
```
