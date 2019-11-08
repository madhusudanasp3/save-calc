<template>
  <div class="sliders">
    <div v-if="!isSubmitted" id="sliders-page">
      <div class="col">
        <div class="row">
          <div class="col">
            <p>Learn how much money and time you'll need to meet your down payment savings goal.</p>
            <p
              class="text-italic"
            >Note: Calculators display default values. Enter new figures to override.</p>
            <p>After entering your Months to Goal (or Monthly Deposit) and Current Savings, adjust the input fields/sliders to determine the Down Payment Cash Needed and the Monthly Deposit (or Months to Goal).</p>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <form action>
              <div class="row">
                <div id="input" class="col col-md-6">
                  <nav>
                    <div id="results-nav-tab" class="nav nav-tabs" role="tablist">
                      <a
                        @click="tab = 0"
                        id="nav-months-to-goal-tab"
                        class="nav-item nav-link"
                        data-toggle="tab"
                        href="#nav-months-to-goal"
                        role="tab"
                        aria-controls="nav-months-to-goal"
                        aria-selected="true"
                      >Months To Goal</a>
                      <a
                        @click="tab = 1"
                        id="nav-monthly-deposit-tab"
                        class="nav-item nav-link"
                        data-toggle="tab"
                        href="#nav-monthly-deposit"
                        role="tab"
                        aria-controls="nav-monthly-deposit"
                        aria-selected="false"
                      >Monthly Deposit</a>
                    </div>
                  </nav>
                  <div id="nav-tabContent" class="tab-content">
                    <div
                      id="nav-months-to-goal"
                      class="tab-pane"
                      :class="{ 'show active': tab === 0 }"
                      role="tabpanel"
                      aria-labelledby="nav-months-to-goal-tab"
                    >
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
                    <div
                      id="nav-monthly-deposit"
                      class="tab-pane"
                      :class="{'show active': tab === 1 }"
                      role="tabpanel"
                      aria-labelledby="nav-monthly-deposit-tab"
                    >
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
                            <div class="input-group-text">/MO</div>
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
                  <div id="current-savings-input-group" class="form-group">
                    <label for="current-savings-input-field">Current Savings</label>
                    <div class="input-group mb-2">
                      <div class="input-group-prepend">
                        <div class="input-group-text">$</div>
                      </div>
                      <input
                        id="current-savings-input-field"
                        v-model.number="$v.currentSavings.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid': $v.currentSavings.$error || $v.currentSavings.$invalid
                        }"
                      />
                      <input
                        id="current-savings-input-range"
                        v-model.number="currentSavings"
                        type="range"
                        class="custom-range"
                        min="0"
                        max="500000"
                      />
                      <div class="invalid-feedback">
                        <span v-if="!$v.currentSavings.between || !$v.currentSavings.required">
                          {{ errorMsgPre }} ${{
                          Number(
                          $v.currentSavings.$params.between.min
                          ).toLocaleString()
                          }}
                          and ${{
                          Number(
                          $v.currentSavings.$params.between.max
                          ).toLocaleString()
                          }}
                        </span>
                      </div>
                    </div>
                  </div>
                  <div id="home-purchase-price-input-group" class="form-group">
                    <label for="home-purchase-price-input-field">Home Purchase Price</label>
                    <div class="input-group mb-2">
                      <div class="input-group-prepend">
                        <div class="input-group-text">$</div>
                      </div>
                      <input
                        id="home-purchase-price-input-field"
                        v-model.number="$v.homePurchasePrice.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.homePurchasePrice.$error || $v.homePurchasePrice.$invalid
                        }"
                      />
                      <input
                        id="home-purchase-price-input-range"
                        v-model.number="homePurchasePrice"
                        type="range"
                        class="custom-range"
                        min="10000"
                        max="1000000"
                      />
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.homePurchasePrice.between || !$v.homePurchasePrice.required
                          "
                        >
                          {{ errorMsgPre }} ${{
                          Number(
                          $v.homePurchasePrice.$params.between.min
                          ).toLocaleString()
                          }}
                          and ${{
                          Number(
                          $v.homePurchasePrice.$params.between.max
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
                    <div id="closing-costs-input-group" class="form-group">
                      <label for="closing-costs-input-field">Closing Costs</label>
                      <div class="input-group mb-2">
                        <div class="input-group-prepend">
                          <div class="input-group-text">$</div>
                        </div>
                        <input
                          id="closing-costs-input-field"
                          v-model.number="$v.closingCosts.$model"
                          type="number"
                          class="form-control"
                          :class="{
                          'is-invalid':
                            $v.closingCosts.$error || $v.closingCosts.$invalid
                        }"
                        />
                        <input
                          id="closing-costs-input-range"
                          v-model.number="closingCosts"
                          type="range"
                          class="custom-range"
                          min="100"
                          max="50000"
                          value="5000"
                        />
                        <div class="invalid-feedback">
                          <span
                            v-if="
                            !$v.closingCosts.between || !$v.closingCosts.required
                          "
                          >
                            {{ errorMsgPre }} ${{
                            Number(
                            $v.closingCosts.$params.between.min
                            ).toLocaleString()
                            }}
                            and ${{
                            Number(
                            $v.closingCosts.$params.between.max
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
                          v-model.number="interestRatePercent"
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
                    <div id="post-closing-reserves-input-group" class="form-group">
                      <label for="post-closing-reserves-input-field">Post Closing Reserves</label>
                      <div class="input-group mb-2">
                        <div class="input-group-prepend">
                          <div class="input-group-text">$</div>
                        </div>
                        <input
                          id="post-closing-reserves-input-field"
                          v-model.number="$v.postClosingReserves.$model"
                          type="number"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              $v.postClosingReserves.$error || $v.postClosingReserves.$invalid
                          }"
                        />
                        <input
                          id="post-closing-reserves-input-range"
                          v-model.number="postClosingReserves"
                          type="range"
                          class="custom-range"
                          min="0"
                          max="20000"
                          value="1869.37"
                        />
                        <div class="invalid-feedback">
                          <span
                            v-if="
                              !$v.postClosingReserves.between ||
                                !$v.postClosingReserves.required
                            "
                          >
                            {{ errorMsgPre }} ${{
                            Number(
                            $v.postClosingReserves.$params.between.min
                            ).toLocaleString()
                            }}
                            and ${{
                            Number(
                            $v.postClosingReserves.$params.between.max
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
                          id="nav-deposit-details-tab"
                          class="nav-item nav-link active"
                          data-toggle="tab"
                          href="#nav-deposit-details"
                          role="tab"
                          aria-controls="nav-deposit-details"
                          aria-selected="true"
                        >Deposit Details</a>
                        <a
                          id="nav-savings-goal-tab"
                          class="nav-item nav-link"
                          data-toggle="tab"
                          href="#nav-savings-goal"
                          role="tab"
                          aria-controls="nav-savings-goal"
                          aria-selected="false"
                        >Savings Goal</a>
                      </div>
                    </nav>
                    <div id="nav-tabContent" class="tab-content">
                      <div
                        id="nav-deposit-details"
                        class="tab-pane fade show active"
                        role="tabpanel"
                        aria-labelledby="nav-deposit-details"
                      >
                        <div class="card-main graph">
                          <div class="card-bottom">
                            <div class="row graph-content">
                              <span class="card-header">Down Payment Cash Needed:</span>
                              <h2>
                                ${{
                                Math.round(maxHomePurchasePrice)
                                }}
                              </h2>
                            </div>
                            <div class="row">
                              <label class="col-8">Monthly Deposit:</label>
                              <span class="col-4 right">${{ Math.round(downPayment) }}</span>
                            </div>
                            <div class="row">
                              <label class="col-8">Months To Goal:</label>
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
                        id="nav-savings-goal"
                        class="tab-pane fade"
                        role="tabpanel"
                        aria-labelledby="nav-savings-goal"
                      >
                        <div class="card-main">
                          <div class="card-top">
                            <span class="card-header">Down Payment Cash Needed:</span>
                            <h2>
                              ${{
                              Math.round(mortgageLoanAmount)
                              }}
                            </h2>
                          </div>
                          <div class="card-bottom">
                            <div class="row">
                              <label class="col-8">Current Savings:</label>
                              <span class="col-4 right">
                                ${{
                                Math.round(totalMonthlyPayment)
                                }}
                              </span>
                            </div>
                            <div class="row">
                              <label class="col-8">Savings Goal:</label>
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
      <p>These calculator results are estimates based on your inputs. Contact a bank, credit union, housing advisor, or lender to determine accurate figures.</p>
    </div>
  </div>
</template>


<script>
import { required, between } from "vuelidate/lib/validators";

const currency = new Intl.NumberFormat("en-US", {
  maximumFractionDigits: 0,
  minimumFractionDigits: 0,
  style: "currency",
  currency: "USD"
});
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
    homePurchasePrice: {
      required,
      between: between(10000, 1000000)
    },
    downPaymentPercent: {
      required,
      between: between(3, 50)
    },
    closingCosts: {
      required,
      between: between(100, 50000)
    },
    interestRatePercent: {
      required,
      between: between(2.5, 11)
    },
    postClosingReserves: {
      required,
      between: between(0, 20000)
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
        homePurchasePrice: 200000,
        downPaymentPercent: 5,
        closingCosts: 5000,
        interestRatePercent: 4.25,
        term: 30,
        postClosingReserves: 1869.37,
        grossIncome: 10,
        carLoan: 500,
        creditCard: 100,
        studentLoan: 150,
        hoi: 1200,
        hoa: 50,
        propertyTax: 1500,
        simpleView: true,
        dti: 0.36,
        closingCostMultiplier: 0.025,
        errorMsgPre: "Please enter a valid amount between",
        tab: 0
      };
    },
    reset() {
      this.monthsToGoal = this.init().monthsToGoal;
      this.depositAmount = this.init().depositAmount;
      this.currentSavings = this.init().currentSavings;
      this.homePurchasePrice = this.init().homePurchasePrice;
      this.downPaymentPercent = this.init().downPaymentPercent;
      this.closingCosts = this.init().closingCosts;
      this.interestRatePercent = this.init().interestRatePercent;
      this.term = this.init().term;
      this.postClosingReserves = this.init().postClosingReserves;
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
    },
    currencyFormat(n) {
      return currency.format(n);
    }
  }
};
</script>
<style lang="sass">
</style>
