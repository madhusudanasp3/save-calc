<template>
  <div class="sliders">
    <div v-if="!isSubmitted" id="sliders-page">
      <div class="col">
        <div class="padding-around">
          <p>Learn how much money and time you'll need to meet your down payment savings goal.</p>
          <p
            class="text-italic"
          >Note: Calculators display default values. Enter new figures to override.</p>
          <p>After entering your Months to Goal (or Monthly Deposit) and Current Savings, adjust the input fields/sliders to determine the Down Payment Cash Needed and the Monthly Deposit (or Months to Goal).</p>
        </div>
        <div class="row">
          <div class="col">
            <form action>
              <div>
                <span
                  class="tab"
                  :class="{ activeTab: selectedTab === tab }"
                  v-for="(tab, index) in tabs"
                  @click="selectedTab = tab"
                  :key="index"
                >{{ tab }}</span>

                <div v-show="selectedTab === 'Months To Goal'">
                  <div class="row">
                    <div id="input" class="col col-md-6">
                      <div id="Months-to-Goal-input-group" class="form-group">
                        <label for="Months-to-Goal-input-field">Months To Goal</label>
                        <div class="input-group mb-2">
                          <input
                            id="Months-to-Goal-input-field"
                            v-model.number="$v.monthsToGoal.$model"
                            type="number"
                            class="form-control"
                            :class="{
                          'is-invalid':
                            $v.monthsToGoal.$error || $v.monthsToGoal.$invalid
                        }"
                          />
                          <input
                            id="Months-to-Goal-input-range"
                            v-model.number="monthsToGoal"
                            type="range"
                            class="custom-range"
                            min="1"
                            max="60"
                            value="10"
                          />
                          <div class="invalid-feedback">
                            <span
                              v-if="
                            !$v.monthsToGoal.between || !$v.monthsToGoal.required
                          "
                            >
                              {{ errorMsgPre }} ${{
                              Number(
                              $v.monthsToGoal.$params.between.min
                              ).toLocaleString()
                              }}
                              and ${{
                              Number(
                              $v.monthsToGoal.$params.between.max
                              ).toLocaleString()
                              }}
                            </span>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div v-show="selectedTab === 'Monthly Deposit'">
                  <div class="row">
                    <div id="input" class="col col-md-6">
                      <div id="deposit-amount-input-group" class="form-group">
                        <label for="deposit-amount-input-field">Deposit Amount</label>
                        <div class="input-group mb-2">
                          <div class="input-group-prepend">
                            <div class="input-group-text">$</div>
                          </div>
                          <input
                            id="deposit-amount-input-field"
                            v-model.number="$v.depositAmount.$model"
                            type="number"
                            class="form-control"
                            :class="{
                          'is-invalid':
                            $v.depositAmount.$error || $v.depositAmount.$invalid
                        }"
                          />
                          <div class="input-group-append">
                            <div class="input-group-text">/mo</div>
                          </div>
                          <input
                            id="deposit-amount-input-range"
                            v-model.number="depositAmount"
                            type="range"
                            class="custom-range"
                            min="10"
                            max="10000"
                            value="1187"
                          />
                          <div class="invalid-feedback">
                            <span
                              v-if="
                            !$v.depositAmount.between || !$v.depositAmount.required
                          "
                            >
                              {{ errorMsgPre }} ${{
                              Number(
                              $v.depositAmount.$params.between.min
                              ).toLocaleString()
                              }}
                              and ${{
                              Number(
                              $v.depositAmount.$params.between.max
                              ).toLocaleString()
                              }}
                            </span>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="row">
                <div id="input" class="col col-md-6">
                  <div id="current-savings-input-group" class="form-group">
                    <label for="current-savings-input-field">Current Savings</label>
                    <div class="input-group mb-2">
                      <div class="input-group-prepend">
                        <div class="input-group-text">$</div>
                      </div>
                      <input
                        id="current-savings-input-field"
                        v-model.number="$v.carLoan.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid': $v.carLoan.$error || $v.carLoan.$invalid
                        }"
                      />
                      <input
                        id="current-savings-input-range"
                        v-model.number="carLoan"
                        type="range"
                        class="custom-range"
                        min="0"
                        max="5000"
                      />
                      <div class="invalid-feedback">
                        <span v-if="!$v.carLoan.between || !$v.carLoan.required">
                          {{ errorMsgPre }} ${{
                          Number(
                          $v.carLoan.$params.between.min
                          ).toLocaleString()
                          }}
                          and ${{
                          Number(
                          $v.carLoan.$params.between.max
                          ).toLocaleString()
                          }}
                        </span>
                      </div>
                    </div>
                  </div>
                  <div id="credit-card-input-group" class="form-group">
                    <label for="credit-card-input-field">Home Purchase Price</label>
                    <div class="input-group mb-2">
                      <div class="input-group-prepend">
                        <div class="input-group-text">$</div>
                      </div>
                      <input
                        id="credit-card-input-field"
                        v-model.number="$v.creditCard.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.creditCard.$error || $v.creditCard.$invalid
                        }"
                      />
                      <input
                        id="credit-card-input-range"
                        v-model.number="creditCard"
                        type="range"
                        class="custom-range"
                        min="0"
                        max="5000"
                      />
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.creditCard.between || !$v.creditCard.required
                          "
                        >
                          {{ errorMsgPre }} ${{
                          Number(
                          $v.creditCard.$params.between.min
                          ).toLocaleString()
                          }}
                          and ${{
                          Number(
                          $v.creditCard.$params.between.max
                          ).toLocaleString()
                          }}
                        </span>
                      </div>
                    </div>
                  </div>
                  <div id="down-payment-input-group" class="form-group">
                    <label for="down-payment-input-field">Down Payment %</label>
                    <div class="input-group mb-2">
                      <input
                        id="down-payment-input-field"
                        v-model.number="$v.downPaymentPercent.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.downPaymentPercent.$error ||
                            $v.downPaymentPercent.$invalid
                        }"
                      />
                      <div class="input-group-append">
                        <div class="input-group-text">%</div>
                      </div>
                      <input
                        id="down-payment-input-range"
                        v-model.number="downPaymentPercent"
                        type="range"
                        class="custom-range"
                        min="3"
                        max="50"
                        step="0.25"
                      />
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.downPaymentPercent.between ||
                              !$v.downPaymentPercent.required
                          "
                        >
                          {{ errorMsgPre }}
                          {{
                          Number(
                          $v.downPaymentPercent.$params.between.min
                          ).toLocaleString()
                          }}% and
                          {{
                          Number(
                          $v.downPaymentPercent.$params.between.max
                          ).toLocaleString()
                          }}%
                        </span>
                      </div>
                    </div>
                  </div>

                  <div v-if="!simpleView">
                    <div id="credit-card-input-group" class="form-group">
                      <label for="credit-card-input-field">closing Costs</label>
                      <div class="input-group mb-2">
                        <div class="input-group-prepend">
                          <div class="input-group-text">$</div>
                        </div>
                        <input
                          id="credit-card-input-field"
                          v-model.number="$v.creditCard.$model"
                          type="number"
                          class="form-control"
                          :class="{
                          'is-invalid':
                            $v.creditCard.$error || $v.creditCard.$invalid
                        }"
                        />
                        <input
                          id="credit-card-input-range"
                          v-model.number="creditCard"
                          type="range"
                          class="custom-range"
                          min="0"
                          max="5000"
                        />
                        <div class="invalid-feedback">
                          <span
                            v-if="
                            !$v.creditCard.between || !$v.creditCard.required
                          "
                          >
                            {{ errorMsgPre }} ${{
                            Number(
                            $v.creditCard.$params.between.min
                            ).toLocaleString()
                            }}
                            and ${{
                            Number(
                            $v.creditCard.$params.between.max
                            ).toLocaleString()
                            }}
                          </span>
                        </div>
                      </div>
                    </div>
                    <div id="interest-rate-input-group" class="form-group">
                      <label for="interest-rate-input-field">Interest Rate</label>
                      <div class="input-group mb-2">
                        <input
                          id="interest-rate-input-field"
                          v-model.number="$v.interestRatePercent.$model"
                          type="number"
                          step="0.25"
                          class="form-control"
                          :class="{
                          'is-invalid':
                            $v.interestRatePercent.$error ||
                            $v.interestRatePercent.$invalid
                        }"
                        />
                        <div class="input-group-append">
                          <div class="input-group-text">%</div>
                        </div>
                        <input
                          id="interest-rate-input-range"
                          v-model.number="interestRate"
                          type="range"
                          class="custom-range"
                          min="2.5"
                          max="11"
                          step="0.25"
                        />
                        <div class="invalid-feedback">
                          <span
                            v-if="
                            !$v.interestRatePercent.between ||
                              !$v.interestRatePercent.required
                          "
                          >
                            {{ errorMsgPre }}
                            {{
                            Number(
                            $v.interestRatePercent.$params.between.min
                            ).toLocaleString()
                            }}% and
                            {{
                            Number(
                            $v.interestRatePercent.$params.between.max
                            ).toLocaleString()
                            }}%
                          </span>
                        </div>
                      </div>
                    </div>
                    <div id="term-input-group" class="form-group">
                      <label for="term-input-select">Term</label>
                      <div class="input-group mb-2">
                        <select id="term-input-select" v-model.number="term" class="form-control">
                          <option>10</option>
                          <option>15</option>
                          <option>20</option>
                          <option>25</option>
                          <option selected>30</option>
                        </select>
                        <div class="input-group-append">
                          <div class="input-group-text">yrs</div>
                        </div>
                        <input
                          id="term-input-range"
                          v-model.number="term"
                          type="range"
                          class="custom-range"
                          min="10"
                          max="30"
                          step="5"
                        />
                      </div>
                    </div>
                    <div id="property-tax-input-group" class="form-group">
                      <label for="property-tax-input-field">Post Closing Reserves</label>
                      <div class="input-group mb-2">
                        <div class="input-group-prepend">
                          <div class="input-group-text">$</div>
                        </div>
                        <input
                          id="property-tax-input-field"
                          v-model.number="$v.propertyTax.$model"
                          type="number"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              $v.propertyTax.$error || $v.propertyTax.$invalid
                          }"
                        />
                        <input
                          id="property-tax-input-range"
                          v-model.number="propertyTax"
                          type="range"
                          class="custom-range"
                          min="400"
                          max="25000"
                        />
                        <div class="invalid-feedback">
                          <span
                            v-if="
                              !$v.propertyTax.between ||
                                !$v.propertyTax.required
                            "
                          >
                            {{ errorMsgPre }} ${{
                            Number(
                            $v.propertyTax.$params.between.min
                            ).toLocaleString()
                            }}
                            and ${{
                            Number(
                            $v.propertyTax.$params.between.max
                            ).toLocaleString()
                            }}
                          </span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div id="result" class="col col-md-6">
                  <div class="sticky">
                    <nav>
                      <div id="results-nav-tab" class="nav nav-tabs" role="tablist">
                        <a
                          id="nav-purchase-info-tab"
                          class="nav-item nav-link active"
                          data-toggle="tab"
                          href="#nav-purchase-info"
                          role="tab"
                          aria-controls="nav-purchase-info"
                          aria-selected="true"
                        >Purchase Info</a>
                        <a
                          id="nav-payment-info-tab"
                          class="nav-item nav-link"
                          data-toggle="tab"
                          href="#nav-payment-info"
                          role="tab"
                          aria-controls="nav-payment-info"
                          aria-selected="false"
                        >Payment Info</a>
                      </div>
                    </nav>
                    <div id="nav-tabContent" class="tab-content">
                      <div
                        id="nav-purchase-info"
                        class="tab-pane fade show active"
                        role="tabpanel"
                        aria-labelledby="nav-purchase-info"
                      >
                        <div class="card-main graph">
                          <div class="card-bottom">
                            <div class="row graph-content">
                              <span class="card-header">Max Home Purchase Price:</span>
                              <h2>
                                ${{
                                Math.round(maxHomePurchasePrice)
                                }}
                              </h2>
                            </div>
                            <div class="row">
                              <label class="col-8">Down Payment:</label>
                              <span class="col-4 right">${{ Math.round(downPayment) }}</span>
                            </div>
                            <div class="row">
                              <label class="col-8">Mortgage Loan Amount:</label>
                              <span class="col-4 right">
                                ${{
                                Math.round(mortgageLoanAmount)
                                }}
                              </span>
                            </div>
                          </div>
                        </div>
                      </div>
                      <div
                        id="nav-payment-info"
                        class="tab-pane fade"
                        role="tabpanel"
                        aria-labelledby="nav-payment-info"
                      >
                        <div class="card-main">
                          <div class="card-top">
                            <span class="card-header">Mortgage Loan Amount:</span>
                            <h2>
                              ${{
                              Math.round(mortgageLoanAmount)
                              }}
                            </h2>
                          </div>
                          <div class="card-bottom">
                            <div class="row">
                              <label class="col-8">MonthlyPayment:</label>
                              <span class="col-4 right">
                                ${{
                                Math.round(totalMonthlyPayment)
                                }}
                              </span>
                            </div>
                            <div class="row">
                              <label class="col-8">Term:</label>
                              <span class="col-4 right">{{ term }} YRS</span>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="col">
                  <hr />
                  <div class="row">
                    <div id="form-mod-group" class="form-group col col-md-6">
                      <button
                        type="button"
                        class="btn btn-base"
                        @click="simpleView = !simpleView"
                      >{{ simpleView ? "Advanced View" : "Simple View" }}</button>
                      <span class="divider">|</span>
                      <button type="button" class="btn btn-base" @click="reset">Reset</button>
                    </div>
                    <div id="submit-form-group" class="form-group col col-md-6">
                      <button
                        class="btn btn-block resultsBtn btn-primary"
                        type="submit"
                        :disabled="$v.$invalid"
                        @click.prevent="isSubmitted = !isSubmitted"
                      >
                        See Results
                        <i class="fa fa-caret-right" />
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="isSubmitted" id="result-page">
      <div class="col">
        <ul class="list-group">
          <p>YOU CAN POTENTIALLY AFFORD A MAX OF:</p>
          <li
            class="list-group-item"
          >Max Home Purchase Price: ${{ Math.round(maxHomePurchasePrice) }}</li>
          <li class="list-group-item">Mortgage Loan Amount: ${{ Math.round(mortgageLoanAmount) }}</li>
          <p>WHAT YOU SHARED ABOUT YOUR FINANCIAL SITUATION:</p>
          <li class="list-group-item">Gross Income: ${{ Math.round(grossIncome) }}</li>
          <li class="list-group-item">Total Debts: ${{ Math.round(totalDebts) }}</li>
          <li class="list-group-item">Car Loans: ${{ Math.round(carLoan) }}</li>
          <li class="list-group-item">Credit Cards: ${{ Math.round(creditCard) }}</li>
          <li class="list-group-item">Student Loans / Other Debts: ${{ Math.round(studentLoan) }}</li>
          <li class="list-group-item">Debt to Income: {{ dtiPercent }}%</li>
          <p>EXPECTED FUNDS NEEDED AT CLOSING:</p>
          <li class="list-group-item">Down Payment: ${{ Math.round(downPayment) }}</li>
          <li class="list-group-item">Closing Costs: ${{ Math.round(closingCost) }}</li>
          <p>INFORMATION ABOUT YOUR POTENTIAL MORTGAGE:</p>
          <li class="list-group-item">Mortgage Loan Amount: ${{ Math.round(mortgageLoanAmount) }}</li>
          <li class="list-group-item">Term: {{ term }} years</li>
          <li class="list-group-item">Interest Rate: {{ interestRatePercent }}%</li>
          <li class="list-group-item">Total Mortgage Payment: ${{ Math.round(totalMonthlyPayment) }}</li>
          <li class="list-group-item">
            Principal Interest and Taxes: ${{
            Math.round(principalInterestPayment)
            }}
          </li>
          <li class="list-group-item">Property Taxes: ${{ Math.round(propertyTaxMonthly) }}</li>
          <li class="list-group-item">Home Owners Insurance: ${{ Math.round(hoiMonthly) }}</li>
          <li class="list-group-item">PMI: ${{ Math.round(monthlyMortagePremium) }}</li>
          <li class="list-group-item">HOA Fees: ${{ Math.round(hoa) }}</li>
        </ul>
      </div>
      <button
        class="btn btn-block btn-primary"
        @click.prevent="isSubmitted = !isSubmitted"
      >Back to Calculator &#8677;</button>
      <p>
        These calculator results are estimates based on your inputs, a 36%* DTI
        (debt-to-income) default setting, and other assumptions required to make
        a preliminary calculation. Depending on your inputs, the default values
        for property taxes, homeowner’s insurance and HOA fees may not be
        accurate for your situation. Contact a bank, credit union, housing
        advisor, or lender to determine your loan eligibility and accurate
        costs. Fannie Mae does not offer mortgage loans to consumers and this in
        no way indicates approval or financing of a mortgage loan.
      </p>
      <p>
        * Lenders often use your DTI and other factors (i.e., underwriting
        guidelines) to confirm eligibility and the type of loan you may be
        offered. Lenders may require a higher or lower DTI percentage to
        determine loan eligibility based on your individual financial situation.
      </p>
    </div>
  </div>
</template>


<script>
import { required, between } from "vuelidate/lib/validators";

export default {
  name: "SaveCalculator",
  data() {
    return this.init();
  },
  validations: {
    monthsToGoal: {
      required,
      between: between(1, 60)
    },
    depositAmount: {
      required,
      between: between(0, 10000)
    },
    currentSavings: {
      required,
      between: between(0, 500000)
    },
    grossIncome: {
      required,
      between: between(1, 24)
    },
    carLoan: {
      required,
      between: between(0, 5000)
    },
    creditCard: {
      required,
      between: between(0, 5000)
    },
    studentLoan: {
      required,
      between: between(0, 5000)
    },
    downPaymentPercent: {
      required,
      between: between(3, 50)
    },
    interestRatePercent: {
      required,
      between: between(2.5, 11)
    },
    hoi: {
      required,
      between: between(0, 24000)
    },
    hoa: {
      required,
      between: between(0, 1000)
    },
    propertyTax: {
      required,
      between: between(0, 40000)
    }
  },
  computed: {
    maxHomePurchasePrice() {
      const max = this.mortgageLoanAmount + this.downPayment;
      return this.handleNegativeNums(max);
    },
    downPayment() {
      const down =
        this.mortgageLoanAmount / (1 - this.downPaymentPercent / 100) -
        this.mortgageLoanAmount;
      return this.handleNegativeNums(down);
    },
    mortgageLoanAmount() {
      const loan = this.pv(
        this.interestRate / 12,
        this.term * 12,
        this.principalInterestPayment
      );
      return this.handleNegativeNums(loan);
    },
    loanToValue() {
      return this.downPayment / this.mortgageLoanAmount;
    },
    estimatedMonthlyPayment() {
      return this.grossIncome * this.dti - this.totalDebts;
    },
    dtiPercent() {
      return this.dti * 100;
    },
    interestRate() {
      return this.interestRatePercent / 100;
    },
    closingCost() {
      return Math.round(this.maxHomePurchasePrice * this.closingCostMultiplier);
    },
    totalMonthlyPayment() {
      const payment =
        this.propertyTaxMonthly +
        this.hoiMonthly +
        this.hoa +
        this.monthlyMortagePremium +
        this.principalInterestPayment;
      if (this.maxHomePurchasePrice <= 0) {
        return 0;
      }
      return this.handleNegativeNums(payment);
    },
    principalInterestPayment() {
      if (this.estimatedMonthlyPayment <= 0) {
        return 0;
      }
      const payment =
        this.estimatedMonthlyPayment -
        (this.propertyTaxMonthly +
          this.hoiMonthly +
          this.hoa +
          this.monthlyMortagePremium);
      return this.handleNegativeNums(payment);
    },
    pmi() {
      if (this.downPaymentPercent >= 20) {
        return 0;
      }
      if (this.downPaymentPercent >= 10) {
        return 0.0044;
      }
      if (this.downPaymentPercent >= 5) {
        return 0.0062;
      }
      return 0.011;
    },
    monthlyMortagePremium() {
      if (this.guessLoanAmount <= 0) {
        return 0;
      }
      const premium = (this.pmi * this.guessLoanAmount) / 12;
      return this.handleNegativeNums(premium);
    },
    guessLoanAmount() {
      return this.pv(
        this.interestRate / 12,
        this.term * 12,
        this.estimatedMonthlyPayment - this.grossIncome * 0.05
      );
    },
    totalDebts() {
      return this.carLoan + this.creditCard + this.studentLoan;
    },
    propertyTaxMonthly() {
      return this.propertyTax / 12;
    },
    hoiMonthly() {
      return this.hoi / 12;
    }
  },
  methods: {
    init() {
      return {
        isSubmitted: false,
        monthsToGoal: 10,
        depositAmount: 1187,
        currentSavings: 5000,
        grossIncome: 10,
        carLoan: 500,
        creditCard: 100,
        studentLoan: 150,
        downPaymentPercent: 5,
        interestRatePercent: 4.25,
        term: 30,
        hoi: 1200,
        hoa: 50,
        propertyTax: 1500,
        simpleView: true,
        dti: 0.36,
        closingCostMultiplier: 0.025,
        errorMsgPre: "Please enter a valid amount between",
        tabs: ["Months To Goal", "Monthly Deposit"],
        selectedTab: "Months To Goal"
      };
    },
    reset() {
      this.monthsToGoal = this.init().monthsToGoal;
      this.depositAmount = this.init().depositAmount;
      this.currentSavings = this.init().depositAmount;
      this.grossIncome = this.init().grossIncome;
      this.carLoan = this.init().carLoan;
      this.creditCard = this.init().creditCard;
      this.studentLoan = this.init().studentLoan;
      this.downPaymentPercent = this.init().downPaymentPercent;
      this.interestRatePercent = this.init().interestRatePercent;
      this.term = this.init().term;
      this.hoi = this.init().hoi;
      this.hoa = this.init().hoa;
      this.propertyTax = this.init().propertyTax;
    },
    pv(rate, nper, pmt) {
      const x = (1 + rate) ** nper;
      return (pmt * ((x - 1) / rate)) / x;
    },
    handleNegativeNums(n) {
      if (n < 0) {
        return 0;
      }
      return n;
    }
  }
};
</script>
<style lang="sass">
</style>
