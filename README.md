import streamlit as st
import pandas as pd

# عنوان اپلیکیشن
st.title("محاسبه‌گر حباب انواع سکه")

# ورودی قیمت طلای 18 عیار
gold_price = st.number_input("قیمت هر گرم طلای ۱۸ عیار (تومان):", min_value=0, value=2700000, step=10000)

# جدول قیمت بازار سکه‌ها
st.subheader("قیمت‌های بازار سکه")

# ورودی قیمت سکه‌ها
coin_prices = {
    "تمام سکه": st.number_input("قیمت تمام سکه (تومان):", min_value=0, value=22500000, step=100000),
    "نیم سکه": st.number_input("قیمت نیم سکه (تومان):", min_value=0, value=12500000, step=100000),
    "ربع سکه": st.number_input("قیمت ربع سکه (تومان):", min_value=0, value=7500000, step=100000),
    "سکه گرمی": st.number_input("قیمت سکه گرمی (تومان):", min_value=0, value=4500000, step=50000),
}

# وزن طلای تقریبی سکه‌ها بر حسب گرم طلای 18 عیار
coin_weights = {
    "تمام سکه": 8.133,
    "نیم سکه": 4.066,
    "ربع سکه": 2.033,
    "سکه گرمی": 1.017,
}

# محاسبه اطلاعات برای نمایش
results = []
for coin, weight in coin_weights.items():
    intrinsic_value = gold_price * weight
    market_price = coin_prices[coin]
    bubble = market_price - intrinsic_value
    suggestion = "خرید طلا" if bubble > 0 else "خرید سکه"
    results.append({
        "نوع سکه": coin,
        "قیمت بازار (تومان)": market_price,
        "ارزش ذاتی (تومان)": int(intrinsic_value),
        "حباب سکه (تومان)": int(bubble),
        "پیشنهاد": suggestion
    })

# نمایش جدول
st.subheader("نتایج")
df = pd.DataFrame(results)
st.dataframe(df, use_container_width=True)
