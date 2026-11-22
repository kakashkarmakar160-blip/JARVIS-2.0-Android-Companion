JARVIS Companion Android Build Patch

এই প্যাকেজে Android Companion-এর জন্য আলাদা, সঠিক Android Gradle project structure এবং GitHub Actions workflow আছে। এটি আপনার মূল website files মুছে বা পরিবর্তন করে না।

ব্যবহার:
1. ZIP extract করুন।
2. এর ভেতরের android-companion ফোল্ডার এবং .github ফোল্ডার আপনার GitHub repository-র root-এ upload করুন (একই নামের ফাইল থাকলে এই patch-এর ফাইল দিয়ে replace করুন)।
3. Commit changes করুন।
4. GitHub repository-তে Actions tab খুলুন।
5. Build JARVIS Companion APK workflow নির্বাচন করে Run workflow চাপুন।
6. Build সফল হলে run-এর Artifacts থেকে jarvis-companion-debug-apk ডাউনলোড করুন।

সীমাবদ্ধতা:
- এটি debug APK build করার প্রাথমিক কাঠামো; Pairing backend-এর সাথে বাস্তব integration এখনো সম্পূর্ণ নয়।
- বর্তমান MainActivity-তে server URL ও bridge token manual input; dashboard pairing code flow এখনো যুক্ত হয়নি।
- notification/location forwarding কেবল Android-এ ব্যবহারকারী অনুমতি দিলে চলবে।
- production release-এর আগে HTTPS endpoint, token issuance/revocation, endpoint contract এবং real-device testing সম্পন্ন করতে হবে।
