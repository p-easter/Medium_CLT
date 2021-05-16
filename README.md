# Medium_CLT
Article on CLT

    # CLT Medium Article Update
    # Standard imports and using random seed 42

    import numpy as np
    import random as ran
    import seaborn as sns
    import matplotlib.pyplot as plt
    from scipy import stats
    plt.style.use('ggplot')
    ran.seed(42)
    np.random.seed(42)
    %matplotlib inline
    plt.style.use('ggplot')

    # Generating synthetic population data with normalized distribution
    population_sbp = np.random.normal(loc=120.0, scale=13, size=100000)
    print('Mean SBP for synthetic population: {}'.format(round(np.mean(population_sbp),2)))

    sns.histplot(population_sbp, kde=True, bins=15).set(title="Popluation SBP Frequency", xlabel="Systolic BP")
    plt.show()

    # Pulling out a cohort of interest
    # Maybe everyone on a stimulant medication
    cohort_bp = [ran.randint(70,200) for i in range(20)]
    print('Mean SBP for synthetic cohorot: {}'.format(round(np.mean(cohort_bp),2)))
    sns.histplot(cohort_bp, kde=True, bins=15).set(xlabel='Systolic BP', title="Cohort SBP Frequency")
    plt.show()

    # Bootstrapping to generate an idea of variance in our cohort
    # n=5, m=40 
    pop_mbp40 = [np.mean(ran.choices(cohort_bp, k=10)) for k in range(40)]

    print('Mean SBP of multiple "Experiments" using bootstrapping of cohort: {}'.format(round(np.mean(pop_mbp40),2)))
    sns.histplot(pop_mbp40, bins=15, kde=True).set(xlabel='Systolic BP', title="Boostrapped Cohort Mean SBP's m=40")

    plt.show()

    # Bootstrapping to generate an idea of variance in our cohort
    # n=5, m=1000 
    pop_mbp1000 = [np.mean(ran.choices(cohort_bp, k=10)) for k in range(1000)]

    print('Mean SBP of multiple "Experiments" using bootstrapping of cohort: {}'.format(round(np.mean(pop_mbp1000),2)))
    sns.histplot(pop_mbp1000, bins=15, kde=True).set(xlabel='Systolic BP', title="Boostrapped Cohort Mean SBP's m=1000")

    plt.show()

    # Bootstrapping to generate an idea of variance in our cohort
    # n=5, m=100000 
    pop_mbp100000 = [np.mean(ran.choices(cohort_bp, k=10)) for k in range(100000)]

    print('Mean SBP of multiple "Experiments" using bootstrapping of cohort: {}'.format(round(np.mean(pop_mbp100000),2)))
    sns.histplot(pop_mbp100000, bins=15, kde=True).set(xlabel='Systolic BP', title="Boostrapped Cohort Mean SBP's m=100000")

    plt.show()

    sns.histplot(pop_mbp1000, cumulative=True, kde=True, stat='density').set(xlabel='Systolic BP', title="CDF Plot")

    # T-test and CI determination
    print(stats.ttest_1samp(pop_mbp1000, np.mean(population_sbp)))
    print(stats.ttest_1samp(pop_mbp1000, np.mean(population_sbp))[1] < 0.05)
    print(np.percentile(pop_mbp10000, [2.5, 97.5]))

    # Running again with simple T-test and larger initial population
    cohort_bp_larger = [ran.randint(70,200) for i in range(100)]
    print(np.mean(cohort_bp))
    print(stats.ttest_1samp(cohort_bp, np.mean(population_sbp)))
